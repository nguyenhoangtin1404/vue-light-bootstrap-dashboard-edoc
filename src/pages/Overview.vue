<template>
  <div class="content">
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-12">
          <div class="card">
            <div class="card-body">
              <div class="row align-items-center">
                <div class="col-md-3">
                  <label for="unitSelect">Đơn vị:</label>
                  <!-- <v-select
                    :options="unitList"
                    v-model="selectedUnit"
                    label="unitName"
                    placeholder="Tìm kiếm đơn vị"
                  ></v-select> -->

    <treeselect v-model="value" :multiple="true" :options="options" />
 
                </div>
                <div class="col-md-3">
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
                <div class="col-md-3">
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
                <div class="col-md-3">
                  <button class="btn btn-primary " @click="fetchData()">
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
        <!-- <div class="col-md-12">
          <chart-card
            :chart-data="lineChart.data"
            :chart-options="lineChart.options"
            :responsive-options="lineChart.responsiveOptions"
          >
            <template slot="header">
              <h4 class="card-title">Tình hỉnh gửi nhận văn bản </h4>
              <p class="card-category">Từ ngày {{ fromDate }} 00:00 đến ngày {{ toDate }} 23:59 </p>
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
        </div> -->

        <div class="col-md-12"> 
        <card class="strpied-tabled-with-hover"  body-classes="table-full-width table-responsive">
            <template slot="header">
              <h4 class="card-title">Tình hình gửi nhận văn bản </h4>
              <p class="card-category">Từ ngày {{ fromDate }} 00:00 đến ngày {{ toDate }} 23:59 </p>
            </template>
            <template>
              <div class="col-md-12" >
                <vue-table-dynamic :params="params"></vue-table-dynamic>
              </div>
            </template>
          </card>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import ChartCard from "src/components/Cards/ChartCard.vue";
import StatsCard from "src/components/Cards/StatsCard.vue";

import axios from "axios";
// Import Vule Datepicker
import DatePick from "vue-date-pick";
import "vue-date-pick/dist/vueDatePick.css";
// Import Vule Select
import vSelect from "vue-select";
import "vue-select/dist/vue-select.css";
// Import Datatable
import VueTableDynamic from 'vue-table-dynamic'
  // import the component
  import Treeselect from '@riophae/vue-treeselect'
  // import the styles
  import '@riophae/vue-treeselect/dist/vue-treeselect.css'

export default {
  components: {
    ChartCard,
    StatsCard,
    DatePick,
    vSelect,
    VueTableDynamic,
    Treeselect 
  },
  data() {
    const currentDate = new Date();
    
    return {
      value: null,
        // define options
        options: [
  {
    "id": "1621",
    "label": "Tỉnh Phú yên",
    "children": [
      {
        "id": "402",
        "label": "Văn phòng Đoàn đại biểu Quốc hội và Hội đồng nhân dân tỉnh",
        "children": null
      },
      {
        "id": "1406",
        "label": "UBND tỉnh Phú Yên",
        "children": [
          {
            "id": "1",
            "label": "UBND TP Tuy Hòa",
            "children": null
          },
          {
            "id": "63",
            "label": "UBND Huyện Sông Hinh - Tỉnh Phú Yên",
            "children": [
              {
                "id": "65",
                "label": "Hội Nông Dân - huyện Sông Hinh",
                "children": null
              }
            ]
          },
          {
            "id": "3",
            "label": "Sở Kế hoạch và Đầu tư",
            "children": null
          }
        ]
      },
      {
        "id": "209",
        "label": "Tỉnh ủy Phú Yên",
        "children": null
      }
    ]
  }
],
      params: {
        data: [],
        header: 'row',
        border: true,
        stripe: true,
        enableSearch: true,
        pagination: true,
        pageSize: 10,
        pageSizes: [5, 10, 20, 50,100],
        sort: [0, 1],
        wordWrap: 'break-word',
        rowHeight: 35,
         columnWidth: [{column: 0, width: '5%'}, {column: 1, width: '55%'}, {column: 2, width: '20%'}, {column: 3, width: '20%'}],
      },
      fromDate: currentDate.toLocaleDateString("en-GB"),
      toDate: currentDate.toLocaleDateString("en-GB"),
      selectedUnit: '', 
      unitName : '0',
      unitList: [],
      send:0,
      received:0,
      countUnit:0,
      // lineChart: {
      //   data: {
      //     labels: [],
      //     series: [],
      //   },
      //   options: {
      //     low: 0,
      //     high: 800,
      //     showArea: false,
      //     height: "245px",
      //     axisX: {
      //       showGrid: false,
      //     },
      //     lineSmooth: true,
      //     showLine: true,
      //     showPoint: true,
      //     fullWidth: true,
      //     chartPadding: {
      //       right: 50,
      //     },
      //   },
      //   responsiveOptions: [
      //     [
      //       "screen and (max-width: 1300px)",
      //       {
      //         axisX: {
      //           labelInterpolationFnc(value) {
      //             return value[0];
      //           },
      //         },
      //       },
      //     ],
      //   ],
      // },
    };
  },
  methods: {
    async fetchData() {
      this.fetchSummary();
      this.fetchDataDetail();
    },
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
      async fetchDataDetail() {
            try {
              const requestData = {
                startDate: this.fromDate,
                endDate: this.toDate,
                unitName: this.selectedUnit ? this.selectedUnit.unitName : this.unitName,
              };

              const response = await axios.post(
                "https://localhost:44315/api/Edocs/DetailDataByDate",
                requestData
              );
              const newData = [['STT','Tên đơn vị', 'Gửi', 'Nhận']];
                 // Xử lý dữ liệu trả về tại đây
              if (response && response.data) {
                response.data.forEach((item, index) => {
                  newData.push([index + 1, item.unitName, item.totalSend, item.totalRece]);
                  //console.log(item);
                });
                this.params.data = newData;
                // Log the updated data for debugging
                //console.log(this.params.data);
              }
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
