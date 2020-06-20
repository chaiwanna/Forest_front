<template>
  <div class="register container">
    <div class="row">
      <div class="col-md-3"></div>
      <div class="col-md-6">
        <div class="card">
          <h1 class="card-header">ข้อมูลส่วนตัว</h1>
          <div class="card-body">
            <form @submit.prevent="onSubmit()">
              <div class="form-group row">
                <label for="inputnumreg" class="col-sm-6 col-form-label">เลขบัตรประจำตัวประชาชน :</label>
                <div class="col-sm-6">
                  <input type="text" v-model.trim="form.NUMREG" class="form-control" />
                </div>
              </div>
              <div class="form-group row">
                <label for="inputmobile" class="col-sm-6 col-form-label">เบอร์โทรศัพท์ :</label>
                <div class="col-sm-6">
                  <input type="text" v-model.trim="form.TEL" class="form-control" />
                </div>
              </div>
              <div class="form-group row">
                <label for="inputname" class="col-sm-6 col-form-label">ชื่อ :</label>
                <div class="col-sm-6">
                   <input type="text" v-model.trim="form.FIRSTNAME" class="form-control" />
                </div>
              </div>
              <div class="form-group row">
                <label for="inputlastname" class="col-sm-6 col-form-label">นามสกุล :</label>
                <div class="col-sm-6">
                   <input type="text" v-model.trim="form.LASTNAME" class="form-control" />
                </div>
              </div>
              <div class="form-group row">
                <label for="inputnickname" class="col-sm-6 col-form-label">ชื่อเล่น :</label>
                <div class="col-sm-6">
                  <input type="text" v-model.trim="form.NICKNAME" class="form-control" />
                </div>
              </div>
              <div class="form-group row">
                <label for="inputblood" class="col-sm-6 col-form-label">หมู่โลหิต :</label>
                <div class="col-sm-6">
                  <input type="text" v-model.trim="form.BLOOD" class="form-control" />
                </div>
              </div>

              <div class="form-group row">
                <label for="inputnumhome" class="col-sm-6 col-form-label">บ้านเลขที่ :</label>
                <div class="col-sm-6">
                  <input type="text" v-model.trim="form.NUMHOME" class="form-control" />
                </div>
              </div>

              <div class="form-group row">
                <label for="inputnummoo" class="col-sm-6 col-form-label">หมู่ที่ :</label>
                <div class="col-sm-6">
                  <input type="text" v-model.trim="form.NUMMOO" class="form-control" />
                </div>
              </div>
              <div class="form-group row">
                <label for="inputprovince" class="col-sm-6 col-form-label">จังหวัด :</label>
                <div class="col-sm-6">
                  <select v-model="form.selected_provinces" class="form-control">
                    <option
                      v-for="item in provinces"
                      :key="item.id"
                      v-bind:value="item.id"
                    >{{item.name_in_thai}}</option>
                  </select>
                </div>
              </div>

                 <div class="form-group row">
                <label for="inputdistricts" class="col-sm-6 col-form-label">อำเภอ :</label>
                <div class="col-sm-6">
                   <select v-model="form.selected_districts" class="form-control">
                    <option
                      v-for="item in districts"
                      :key="item.id"
                      v-bind:value="item.id"
                    >{{item.name_in_thai}}</option>
                  </select>
                </div>
              </div>

              <div class="form-group row">
                <label for="inputsubdistricts" class="col-sm-6 col-form-label">ตำบล :</label>
                <div class="col-sm-6">
                   <select v-model="form.selected_subdistricts" class="form-control">
                    <option
                      v-for="item in subdistricts"
                      :key="item.id"
                      v-bind:value="item.id"
                    >{{item.name_in_thai}}</option>
                  </select>
                </div>
              </div>

              <div class="form-group row">
                <label for="inputpostman" class="col-sm-6 col-form-label">รหัสไปรษณีย์ :</label>
                <div class="col-sm-6">
                  <input type="text" v-model.trim="form.ZIPCODE" class="form-control" />
                </div>
              </div>

              <div class="forn-group">
                <button type="submit" class="btn btn-success btn-block">บันทึก</button>
                <button
                  type="button"
                  @click="onRedirectToHome()"
                  class="btn btn-secondary btn-block"
                >ยกเลิก</button>
              </div>
            </form>
          </div>
        </div>
      </div>
      <div class="col-md-3"></div>
    </div>
  </div>
</template>


<script>
import axios from 'axios';
import _ from 'lodash';
import { mapState } from 'vuex';
import User from '../../model/user';

export default {
  name: 'information',
  computed: {
    ...mapState('auth', ['user'])
  },
  data() {
    return {
      form: {
        NUMREG: null,
        TEL: null,
        FIRSTNAME: null,
        LASTNAME: null,
        NICKNAME: null,
        BLOOD: null,
        NUMHOME: null,
        NUMMOO: null,
        selected_provinces: 0,
        selected_districts: 0,
        selected_subdistricts: 0,
        ZIPCODE: null
      },
      provinces: [{ id: 0, name_in_thai: 'เลือกจังหวัด' }],
      districts: [{ id: 0, name_in_thai: 'เลือกอำเภอ' }],
      subdistricts: [{ id: 0, name_in_thai: 'เลือกตำบล' }]
    
    };
  },
mounted(){
  this.getUser(this.user.id);
  this.getProvinces();
  this.getDistricts(this.form.selected_provinces);
  this.getSubdistricts(this.form.selected_districts);
   
},
  methods: {
    showNavigation(permissionKey) {
      if (this.user.role === User.userRole.administrator) {
        return true;
      }
      return _.includes(this.user.permission_keys, permissionKey);
    },
    getUser(id) {
      axios.get(`http://localhost:3000/staff/${id}`).then(response => {
        // this.user = response.data.data;
         this.form.NUMREG = response.data.data.numreg;
         this.form.TEL = response.data.data.tel;
         this.form.FIRSTNAME = response.data.data.first_name;
         this.form.LASTNAME = response.data.data.last_name;
         this.form.NICKNAME = response.data.data.nick_name;
         this.form.BLOOD = response.data.data.blood;
         this.form.NUMHOME = response.data.data.numhome;
         this.form.NUMMOO = response.data.data.nummoo;
         this.form.selected_districts = response.data.data.districts;
         this.form.selected_subdistricts = response.data.data.subdistricts;
         this.form.selected_provinces = response.data.data.province;
         this.form.ZIPCODE = response.data.data.postman;
         
        console.log(response.data);
      });
    },
      updateUser(form,userid) {
      axios.patch(`http://localhost:3000/staff/${userid}`,form).then(response => {
        // this.user = response.data.data;
        console.log(response);
      });
    },
      getProvinces() {
      axios.get('http://localhost:3000/country/provinces').then(response => {
        this.provinces = this.provinces.concat(response.data.data);
        console.log(this.provinces);
      });
    },
    getDistricts(id) {
      axios.get(`http://localhost:3000/country/districts/${id}`).then(response => {
        this.districts = this.districts.concat(response.data.data);
         console.log(this.districts);
      });
    },
    getSubdistricts(id) {
      axios.get(`http://localhost:3000/country/subdistricts/${id}`).then(response => {
        // console.log(response);
        this.subdistricts = this.subdistricts.concat(response.data.data);
        console.log(this.subdistricts);
      });
    },

    onSubmit() {
      const updateUser = {
        username: this.form.username,
        email: this.form.email,
        first_name: this.form.FIRSTNAME,
        last_name: this.form.LASTNAME,
        nick_name: this.form.NICKNAME,
        numreg:  this.form.NUMREG,
        tel: this.form.TEL,
        numhome: this.form.NUMHOME,
        nummoo: this.form.NUMMOO,
        province: this.form.selected_provinces,
        districts: this.form.selected_districts,
        subdistricts: this.form.selected_subdistricts,
        postman: this.form.ZIPCODE,
        password: this.form.password,
        confirmed_at: this.form.confirmedAt,
        blocked_at: this.form.blockedAt,
        role: this.form.role ,
        permissions: this.form.permissions,
        enabled: this.form.enabled
      };
     this.updateUser(updateUser,this.user.id);
      //  window.location.reload();
      // console.log(this.form);
      
      // console.log(updateUser);
      
     
        // this.$emit('add', { user });
    
      return false;
      }
  },
   watch: {
    'form.selected_provinces': function(val) {
      this.districts = [];
      this.getDistricts(val);
    },
    'form.selected_districts': function(val) {
      this.subdistricts = [];
      this.getSubdistricts(val);
    }
    }

};
    // ไปหน้าลงทะเบียน
    // onRedirectToHome() {
    //   this.$router.push("/");
    // }
 

</script>
<style scoped>
.name-system {
  width: 80%;
  display: block;
  margin: auto;
  margin-top: 15%;
  margin-bottom: 40px;
}
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
.btn {
  margin-top: 30px;
}
</style>