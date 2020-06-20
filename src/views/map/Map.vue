<template>
  <div>
    <div class="row">
      <div class="col-md-1"></div>
      <div class="col-md-10">
        <div class="card">
          <h1 class="card-header">แผนที่</h1>
          <div class="card-body">
            <div>
               <div class="row">
              <div class="col-xl-12">

                <div class="form-group row">
                  <label for="inputsubdistricts" class="col-sm-2 col-form-label">เลือกช่วงเวลา :</label>
                  <div class="col-sm-3">
                    <date-picker v-model="form.time3" range></date-picker>
                  </div>
                  <b-button class="col-sm-2" type="submit" size="sm" variant="success" v-on:click="getForest()">
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
              <div class="container">
                <div class="row justify-content-md-center">
                  <div class="col-xl-12">
                    <div id="app">
                      <longdo-map :center="center" :zoom="10" :lastView="false">
                        <longdo-map-marker
                          v-for="(item, i) in markers"
                          :key="i"
                          :location="item.location"
                          :title="item.title"
                          :detail="item.detail"
                        />
                      </longdo-map>
                    </div>
                  </div>
                </div>
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
import { mapState } from 'vuex';
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import 'vue2-datepicker/locale/es/th';
import { LongdoMap, LongdoMapMarker } from 'longdo-map-vue';
import reportService from '../../services/reportService';
import userService from '../../services/newUserService';


export default {
  name: 'map',
  component: {
    'longdo-map': LongdoMap,
    LongdoMapMarker
  },
  components:{
    DatePicker
  },
  computed: {
    ...mapState('auth', ['user'])
  },
  data() {
    return {
      markers: [],
      center: {
        lon: 19.1,
        lat: 100.1
      },
      form: {
        search: null,
        time3: null,
        user_id:null
      },
      userList : []

    };
  },
  methods: {
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
    async getForest() {
      this.markers = []
      const map = (await reportService.getMapDetail(this.createFilter())).data;
      map.forEach(element => {
        this.markers.push({
          location: { lon: element.longitude, lat: element.latitude },
          title: element.name,
          detail: `จำนวนคนที่เข้าใช้ ${element.count}`
        });
      });
    },async loadUserList(){
      const data = await userService.getAll();
      this.userList = data.data
    }
  },
  mounted() {
    this.getForest();
    this.loadUserList();
  }
};
</script>

<style>
#app {
  width: 100%;
  height: 600px;
}
</style>