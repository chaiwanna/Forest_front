<template>
  <div class="history">
    <Layout>
      <!-- <b-form-group id="group-search " label-for="input-search ">
          <b-form-input
            id="input-search"
            v-model="form.search"
            type="text"
            placeholder="ค้นหาข้อมูล"
          ></b-form-input>
      </b-form-group>-->
      <div class="row">
        <div class="col-xl-4">
          <date-picker v-model="form.time3" range v-on:change="showTime()"></date-picker>
        </div>
        <div class="col-xl-2">
          <b-button type="submit" size="sm" variant="success" v-on:click="loadList()">
            <font-awesome-icon :icon="['fas', 'save']" class="mr-1" />ค้นหา
          </b-button>
        </div>
      </div>
      <br />
      <div class="card">
        <h1 class="card-header">
          ประวัติการเข้าป่าไม้
          <b-button size="sm" variant="primary" v-on:click="onDownload()">
            <font-awesome-icon icon="download" class="mr-1" />ดาวน์โหลด Excel
          </b-button>
        </h1>
        <div class="card-body">
          <table class="table">
            <thead>
              <tr>
                <th>ชื่อ-นามสกุล</th>
                <th>เลขประจำตัวประชาชน</th>
                <th>ที่อยู่</th>
                <th>วัตถุประสงค์ในการเข้าไปในพื้นที่ป่า</th>
                <th>วันที่เข้าป่า</th>
                <th>เวลาที่เข้าป่า</th>
                <th>จัดการ</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in dataPaginate" :key="index">
                <td>{{item.user[0].first_name}} {{item.user[0].last_name}}</td>
                <td>{{item.user[0].numreg}}</td>
                <td>ที่อยู่</td>
                <td>{{item.objective}}</td>
                <td>{{formateDate(item.time)}}</td>
                <td>{{formateTime(item.time)}}</td>
                <td>
                  <!-- <b-button size="sm" variant="danger">
                    <font-awesome-icon icon="trash" class="mr-1" />
                  </b-button>-->

                  <b-button size="sm" variant="danger">
                    <font-awesome-icon icon="trash" class="mr-1" />
                  </b-button>

                  <b-button size="sm" variant="danger">
                    <font-awesome-icon icon="trash" class="mr-1" />
                  </b-button>
                </td>
              </tr>
            </tbody>
          </table>
          <!-- paginate -->
          <!-- <nav aria-label="Page navigation example">
            <ul class="pagination justify-content-end">
              <li class="page-item disabled">
                <a class="page-link" href="#" tabindex="-1" aria-disabled="true">Previous</a>
              </li>
              <li class="page-item">
                <a class="page-link" href="#">1</a>
              </li>
              <li class="page-item">
                <a class="page-link" href="#">2</a>
              </li>
              <li class="page-item">
                <a class="page-link" href="#">3</a>
              </li>
              <li class="page-item">
                <a class="page-link" href="#">Next</a>
              </li>
            </ul>
          </nav>-->
          <!-- end paginate -->
        </div>
      </div>
    </Layout>
  </div>
</template>

<script>
import { mapState } from 'vuex';
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import 'vue2-datepicker/locale/es/th';
import moment from 'moment';
import Layout from '../../components/Layout.vue';
import forestAccessService from '../../services/forestAccessService';
import reportService from '../../services/reportService';

export default {
  name: 'History',
  components: {
    Layout,
    DatePicker
  },
  computed: {
    ...mapState('auth', ['user'])
  },
  data() {
    return {
      form: {
        search: null,
        time3: null
      },
      dataPaginate: []
    };
  },
  methods: {
    async getDataPaginate(filter = {}) {
      this.dataPaginate = (await forestAccessService.getPaginate(filter)).data.data;
    },
    formateDate(date) {
      return moment(String(date)).format('MM/DD/YYYY');
    },
    formateTime(date) {
      return moment(String(date)).format('hh:mm');
    },
    showTime() {
      console.log(this.form);
    },
    onDownload() {
      let a;
      if (this.user.role === 99) {
        reportService.getExcel(JSON.stringify(this.createFilter())).then(res => {
          window.open(res);
        });
      } else {
        reportService.getExcel(JSON.stringify(this.createFilter())).then(res => {
          window.open(res);
        });
      }
      return a;
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
      if (this.user.role !== 99) {
        filter.user_id = this.user.id;
        return {'filter':filter};
      }
      return {'filter':filter};
    },
    loadList() {
      if (this.user.role === 99) {
        this.getDataPaginate(this.createFilter());
      } else {
        this.getDataPaginate(this.createFilter());
      }
    }
  },
  created() {},
  mounted() {
    this.createFilter();
    this.loadList();
  },

  watch: {}
};
</script>
<style>
.card-header {
  font-size: 20px;
}
</style>
