<template>
  <div>
    <div class="row">
      <div class="col-md-1"></div>
      <div class="col-md-10">
        <div class="card">
          <h1 class="card-header">รายงานจำนวนคนเข้าป่า</h1>
          <div class="card-body">
            <div class="row">
              <div class="col-xl-12">

                <div class="form-group row">
                  <label for="inputsubdistricts" class="col-sm-2 col-form-label">เลือกช่วงเวลา :</label>
                  <div class="col-sm-3">
                    <date-picker v-model="form.time3" range></date-picker>
                  </div>
                  <b-button class="col-sm-2" type="submit" size="sm" variant="success" v-on:click="loadData()">
                  <font-awesome-icon :icon="['fas', 'save']" class="mr-1" />ค้นหา
                </b-button>
                </div>

                <div class="form-group row">
                  <label for="inputsubdistricts" class="col-sm-2 col-form-label">ค้นหาตามบุคคล :</label>
                  <div class="col-sm-3">
                    <select v-model="form.user_id" class="form-control">
                      <option
                        v-for="item in userList"
                        :key="item.id"
                        v-bind:value="item.id"
                      >{{item.first_name}} {{item.last_name}}</option>
                    </select>
                  </div>
                  
                </div>

              </div>
              
            </div>
            <div class="row">
              <div class="col-md-12">
                <GChart type="LineChart" :data="chartData" :options="chartOptions" />
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-1"></div>
    </div>
  </div>
</template>

<script>
import DatePicker from 'vue2-datepicker';
import { GChart } from 'vue-google-charts';
import reportService from '../../services/reportService';
import userService from '../../services/newUserService';

export default {
  components: {
    GChart,
    DatePicker
  },
  data() {
    return {
      form: {
        search: null,
        time3: null,
        user_id:null
      },
      userList : [],
      // Array will be automatically processed with visualization.arrayToDataTable function
      date: new Date(),
      chartData: [['เวลา', 'จำนวน']],
      chartOptions: {
        chart: {
          title: 'จำนวนการเข้าป่า'
          // subtitle: 'Sales, Expenses, and Profit: 2014-2017'
        },
        vAxis: { minValue: 0 }
      }
    };
  },
  mounted() {
    this.loadData();
    this.loadUserList();
  },
  methods: {
    async loadData() {
      const data = await reportService.getGraph(this.createFilter());
      console.log(data.data);
      this.chartData = [['เวลา', 'จำนวน']];
      data.data.forEach(element => {
        this.chartData.push([element.time, element.count]);
      });
      console.log(this.chartData);
    },
    createFilter() {
      const filter = {};
      if (this.form.time3) {
        const [one, two] = this.form.time3;
        if (one) {
          filter.date_from = one;
        }
        if (two) {
          filter.date_too = two;
        }
      }
      if (this.form.user_id) {
          filter.user_id = this.form.user_id;
        
      }
      const returnData = { filter };
      return returnData;
    },
    async loadUserList(){
      const data = await userService.getAll();
      this.userList = data.data
    }
  }
};
</script>

<style scoped>
h1 {
  font-size: 18px;
  font-weight: 600;
  text-align: center;
}
.card-header {
  letter-spacing: 5px;
}
.card-body {
  padding-left: 5%;
  padding-right: 5%;
}
</style>