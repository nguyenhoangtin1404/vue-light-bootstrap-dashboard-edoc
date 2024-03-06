<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-12">
          <div class="card">
            <div class="card-body">
              <div class="row align-items-center">
                <div class="col-md-2">
                  <label for="unitSelect">Đơn vị:</label>
                  <v-select
                    :options="unitList"
                    v-model="selectedUnit"
                    label="unitName"
                    placeholder="Tìm kiếm đơn vị"
                  ></v-select>
                </div>
                <div class="col-md-2">
                  <label for="fromDate">Từ ngày:</label></br>
                  <template>
                    <date-pick
                      v-model="fromDate"
                      :format="'DD/MM/YYYY'"
                      :inputAttributes="{ readonly: true }"
                      id="fromDate"
                    ></date-pick>
                  </template>
                </div>
                <div class="col-md-2">
                  <label for="toDate">Đến ngày:</label></br>
                  <template>
                    <date-pick
                      v-model="toDate"
                      :format="'DD/MM/YYYY'"
                      :inputAttributes="{ readonly: true }"
                      id="toDate"
                    ></date-pick>
                  </template>
                </div>
                <div class="col-md-2">
                  <button class="btn btn-primary mt-4 mt-md-0" @click="fetchSummary">
                    Xem
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-xl-3 col-md-6">
          <stats-card>
            <div slot="header" class="icon-warning">
              <i class="nc-icon nc-single-copy-04 text-warning"></i>
            </div>
            <div slot="content">
              <p class="card-category">Tổng số văn bản gửi - nhận</p>
              <h4 class="card-title">{{ send + received }} Văn bản</h4>
            </div>
            <div slot="footer"><i class="fa fa-refresh"></i>Updated now</div>
          </stats-card>
        </div>

        <div class="col-xl-3 col-md-6">
          <stats-card>
            <div slot="header" class="icon-success">
              <i class="nc-icon nc-single-copy-04 text-success"></i>
            </div>
            <div slot="content">
              <p class="card-category">Tổng số văn bản gửi</p>
              <h4 class="card-title">{{ send  }} Văn bản</h4>
            </div>
            <div slot="footer"><i class="fa fa-calendar-o"></i>Last day</div>
          </stats-card>
        </div>

        <div class="col-xl-3 col-md-6">
          <stats-card>
            <div slot="header" class="icon-danger">
              <i class="nc-icon nc-single-copy-04 text-danger"></i>
            </div>
            <div slot="content">
              <p class="card-category">Tổng số văn bản nhận</p>
              <h4 class="card-title">{{ received }} Văn bản</h4>
            </div>
            <div slot="footer"><i class="fa fa-clock-o"></i>Last day</div>
          </stats-card>
        </div>

        <div class="col-xl-3 col-md-6">
          <stats-card>
            <div slot="header" class="icon-info">
              <i class="nc-icon nc-single-02 text-primary"></i>
            </div>
            <div slot="content">
              <p class="card-category">Số lượng đơn vị</p>
              <h4 class="card-title">{{ countUnit  }} Đơn vị</h4>
            </div>
            <div slot="footer"><i class="fa fa-refresh"></i>Updated now</div>
          </stats-card>
        </div>
      </div>
      <div class="row">
        <div class="col-md-12">
          <chart-card
            :chart-data="lineChart.data"
            :chart-options="lineChart.options"
            :responsive-options="lineChart.responsiveOptions"
          >
            <template slot="header">
              <h4 class="card-title">Users Behavior</h4>
              <p class="card-category">24 Hours performance</p>
            </template>
            <template slot="footer">
              <div class="legend">
                <i class="fa fa-circle text-info"></i> Tổng gửi - nhận
                <i class="fa fa-circle text-danger"></i> Gửi
                <i class="fa fa-circle text-warning"></i> Nhận
              </div>
              <hr />
              <div class="stats">
                <i class="fa fa-history"></i> Updated 3 minutes ago
              </div>
            </template>
          </chart-card>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import ChartCard from "src/components/Cards/ChartCard.vue";
import StatsCard from "src/components/Cards/StatsCard.vue";
import LTable from "src/components/Table.vue";
import axios from "axios";
// Import Vule Datepicker
import DatePick from "vue-date-pick";
import "vue-date-pick/dist/vueDatePick.css";
// Import Vule Select
import vSelect from "vue-select";
import "vue-select/dist/vue-select.css";

export default {
  components: {
    LTable,
    ChartCard,
    StatsCard,
    DatePick,
    vSelect,
  },
  data() {
    const currentDate = new Date();
    return {
      fromDate: currentDate.toLocaleDateString("en-GB"),
    toDate: currentDate.toLocaleDateString("en-GB"),
      selectedUnit: "", 
      unitName : '0',
      unitList: [],
      send:0,
      received:0,
      countUnit:0,
      lineChart: {
        data: {
          labels: [
            "9:00AM",
            "12:00AM",
            "3:00PM",
            "6:00PM",
            "9:00PM",
            "12:00PM",
            "3:00AM",
            "6:00AM",
          ],
          series: [
            [287, 385, 490, 492, 554, 586, 698, 695],
            [67, 152, 143, 240, 287, 335, 435, 437],
            [23, 113, 67, 108, 190, 239, 307, 308],
          ],
        },
        options: {
          low: 0,
          high: 800,
          showArea: false,
          height: "245px",
          axisX: {
            showGrid: false,
          },
          lineSmooth: true,
          showLine: true,
          showPoint: true,
          fullWidth: true,
          chartPadding: {
            right: 50,
          },
        },
        responsiveOptions: [
          [
            "screen and (max-width: 1300px)",
            {
              axisX: {
                labelInterpolationFnc(value) {
                  return value[0];
                },
              },
            },
          ],
        ],
      },
    };
  },
  methods: {
    async fetchUnits() {
      try {
        const response = await axios.get(
          "https://localhost:44315/api/edocs/GetUnits"
        );
        this.unitList = response.data;
      } catch (error) {
        console.error("Lỗi khi lấy dữ liệu đơn vị:", error);
      }
    },
    async fetchSummary() {
        try {
          const requestData = {
            startDate: this.fromDate,
            endDate: this.toDate,
            unitName: this.selectedUnit ? this.selectedUnit.unitName : this.unitName,
          };

          const response = await axios.post(
            "https://localhost:44315/api/Edocs/SummaryByDate",
            requestData
          );

          // Xử lý dữ liệu trả về tại đây
          if (response && response.data) {
            this.send = response.data[0].send;
            this.received = response.data[0].received;
            this.countUnit = response.data[0].countUnit;
          }
        } catch (error) {
          console.error("Lỗi khi gọi API:", error);
        }
      },
    async fetchData() {
          try {
            const requestData = {
              startDate: this.fromDate,
              endDate: this.toDate,
              unitName: this.selectedUnit? this.selectedUnit.unitName : this.unitName,
            };

            const response = await axios.post(
              "https://localhost:44315/api/Edocs/FilterByDate",
              requestData
            );

            // Xử lý dữ liệu trả về tại đây
          } catch (error) {
            console.error("Lỗi khi gọi API:", error);
          }
    },
  },
  mounted() {
    // Gọi hàm fetchUnits khi component được mount
    this.fetchUnits();
  },
};
</script>
