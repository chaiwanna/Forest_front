<template>
<div>
    <div class="row">
      <div class="col-md-1"></div>
      <div class="col-md-10">
        <div class="card">
          <h1 class="card-header">Dashboard</h1>
          <div class="card-body">
           <div class="page-class page-home">
    <div class="row justify-content-md-center">
      <h1>{{date.day}} {{month[date.month]}} {{date.year}}</h1>
    </div>
    <!-- card section -->
    <div class="row">
      <div class="col-xl-3 col-sm-6 mb-3">
        <div class="card text-white bg-primary o-hidden h-100">
          <div class="card-body">
            <div class="card-body-icon">
              <font-awesome-icon icon="user-circle" />
            </div>
            <div class="mr-5">
              <h3>จำนวนคนเข้าป่า</h3>
            </div>
            <div class="mr-5">
              <h1>{{dashboard.dashboard_access_per_day}} คน</h1>
            </div>
          </div>
          <router-link  class="card-footer text-white clearfix small z-1" :to="{path : '/report'}">
            <span class="float-left">View Details</span>
            <span class="float-right">
              <i class="fa fa-angle-right"></i>
            </span>
          </router-link>
        </div>
      </div>
      <div class="col-xl-3 col-sm-6 mb-3">
        <div class="card text-white bg-warning o-hidden h-100">
          <div class="card-body">
            <div class="card-body-icon">
              <font-awesome-icon icon="tree" />
            </div>
            <div class="mr-5">
              <h3>จำนวนป่าที่ดูแล</h3>
            </div>
            <div class="mr-5">
              <h1>{{dashboard.dashboard_forest_care}} พื้นที่</h1>
            </div>
          </div>
          <router-link class="card-footer text-white clearfix small z-1" :to="{path : '/map'}">
            <span class="float-left">View Details</span>
            <span class="float-right">
              <i class="fa fa-angle-right"></i>
            </span>
          </router-link>
        </div>
      </div>
      <!-- more card style -->
      <!-- <div class="col-xl-3 col-sm-6 mb-3">
        <div class="card text-white bg-success o-hidden h-100">
          <div class="card-body">
            <div class="card-body-icon">
              <i class="fa fa-fw fa-shopping-cart"></i>
            </div>
            <div class="mr-5">123 New Orders!</div>
          </div>
          <a class="card-footer text-white clearfix small z-1" href="#">
            <span class="float-left">View Details</span>
            <span class="float-right">
              <i class="fa fa-angle-right"></i>
            </span>
          </a>
        </div>
      </div>
      <div class="col-xl-3 col-sm-6 mb-3">
        <div class="card text-white bg-danger o-hidden h-100">
          <div class="card-body">
            <div class="card-body-icon">
              <i class="fa fa-fw fa-support"></i>
            </div>
            <div class="mr-5">13 New Tickets!</div>
          </div>
          <a class="card-footer text-white clearfix small z-1" href="#">
            <span class="float-left">View Details</span>
            <span class="float-right">
              <i class="fa fa-angle-right"></i>
            </span>
          </a>
        </div>
      </div> -->
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
import reportService from '../services/reportService';

export default {
  name: 'Home',
  components: {},
  metaInfo: {
    title: 'Home',
    meta: [
      {
        name: 'description',
        content: 'Welcome to Vue.js backend.'
      }
    ]
  },
  data() {
    return {
      dashboard: {
        dashboard_access_per_day: 0,
        dashboard_forest_care: 0
      },
      month:[
        'มกราคม',
        'กุมภาพันธ์',
        'มีนาคม',
        'เมษายน',
        'พฤษภาคม',
        'มิถุนายน',
        'กรกฎาคม',
        'สิงหาคม',
        'กันยายน',
        'ตุลาคม',
        'พฤศจิกายน',
        'ธันวาคม'
      ]
      ,
      date: {
        day : new Date().getDate(),
        month : new Date().getMonth(),
        year : new Date().getFullYear()+543
      }
      
    };
  },
  methods: {
    async getDashboard() {
      this.dashboard = (await reportService.getReport()).data;
    }
  },
  mounted() {
    this.getDashboard();
  }
};
</script>

<style>
.card-body-icon {
  position: absolute;
  z-index: 0;
  top: -0px;
  right: 10px;
  font-size: 5rem;
  /* -webkit-transform: rotate(15deg); */
  /* -ms-transform: rotate(15deg); */
  /* transform: rotate(15deg); */
}

@media (min-width: 576px) {
  .card-columns {
    column-count: 1;
  }
}

@media (min-width: 768px) {
  .card-columns {
    column-count: 2;
  }
}

@media (min-width: 1200px) {
  .card-columns {
    column-count: 2;
  }
}
</style>
