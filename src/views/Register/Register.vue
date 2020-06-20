<template>
  <div class="register container">
    <div class="row">
      <div class="col-md-3"></div>
      <div class="col-md-6">
        <h1 class="name-system">ระบบลงทะเบียนผู้เข้าใช้พื้นที่เขตรักษาพันธุ์สัตว์ป่าเวียงลอ</h1>
        <div class="card">
          <h1 class="card-header">REGISTER</h1>
          <div class="card-body">
            <div v-if="loading || !formLoaded">
              <span class="spinner"></span>
            </div>
            <div v-if="!loading && formLoaded">
              <b-form @submit.stop.prevent="onSubmit">
                <b-form-group id="group-username" label-for="input-username">
                  <template v-slot:label>
                    ชื่อผู้ใช้งาน
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-username"
                    v-model="form.username"
                    type="text"
                    placeholder="ชื่อผู้ใช้งาน"
                    :state="$v.form.username.$dirty ? !$v.form.username.$error : null"
                  ></b-form-input>

                  <b-form-invalid-feedback id="input-username-invalid">โปรดกรอกชื่อผู้ใช้งาน</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-first-name" label-for="input-first-name">
                  <template v-slot:label>
                    ชื่อ
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-first-name"
                    type="text"
                    v-model="form.firstName"
                    :state="$v.form.firstName.$dirty ? !$v.form.firstName.$error : null"
                    placeholder="ชื่อ"
                  ></b-form-input>

                  <b-form-invalid-feedback
                    id="input-first-name-invalid"
                  >Please enter your first name.</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-last-name" label-for="input-last-name">
                  <template v-slot:label>
                    นามสกุล
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-last-name"
                    type="text"
                    v-model="form.lastName"
                    :state="$v.form.lastName.$dirty ? !$v.form.lastName.$error : null"
                    placeholder="นามสกุล"
                  ></b-form-input>

                  <b-form-invalid-feedback id="input-last-name-invalid">โปรดกรอกนามสกุล</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-nick-name" label-for="input-nick-name">
                  <template v-slot:label>
                    ชื่อเล่น
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-nick-name"
                    type="text"
                    v-model="form.nickname"
                    :state="$v.form.nickname.$dirty ? !$v.form.nickname.$error : null"
                    placeholder="ชื่อเล่น"
                  ></b-form-input>

                  <b-form-invalid-feedback id="input-nick-name-invalid">โปรดกรอกชื่อเล่น</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-num-reg" label-for="input-num-reg">
                  <template v-slot:label>
                    เลขบัตรประจำตัวประชาชน
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-num-reg"
                    type="text"
                    v-model="form.numreg"
                    :state="$v.form.numreg.$dirty ? !$v.form.numreg.$error : null"
                    placeholder="เลขบัตรประจำตัวประชาชน"
                  ></b-form-input>

                  <b-form-invalid-feedback
                    id="input-last-name-invalid"
                  >โปรดกรอกเลขบัตรประจำตัวประชาชน</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-num-tel" label-for="input-num-tel">
                  <template v-slot:label>
                    เบอร์โทรศัพท์
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-num-tel"
                    type="text"
                    v-model="form.tel"
                    :state="$v.form.tel.$dirty ? !$v.form.tel.$error : null"
                    placeholder="เบอร์โทรศัพท์"
                  ></b-form-input>

                  <b-form-invalid-feedback id="input-num-tel-invalid">โปรดกรอกเบอร์โทรศัพท์</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-num-home" label-for="input-num-home">
                  <template v-slot:label>
                    บ้านเลขที่
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-num-home"
                    type="text"
                    v-model="form.numhome"
                    :state="$v.form.numhome.$dirty ? !$v.form.numhome.$error : null"
                    placeholder="บ้านเลขที่"
                  ></b-form-input>

                  <b-form-invalid-feedback id="input-last-name-invalid">โปรดกรอกบ้านเลขที่</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-num-moo" label-for="input-num-moo">
                  <template v-slot:label>
                    หมู่ที่
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-num-moo"
                    type="text"
                    v-model="form.nummoo"
                    :state="$v.form.nummoo.$dirty ? !$v.form.nummoo.$error : null"
                    placeholder="หมู่ที่"
                  ></b-form-input>

                  <b-form-invalid-feedback id="input-last-name-invalid">โปรดกรอกเลขที่หมู่บ้าน</b-form-invalid-feedback>
                </b-form-group>

                <!-- จังหวัด -->
                <b-form-group id="group-num-provinces" label-for="input-num-provinces">
                  <template v-slot:label>
                    จังหวัด
                    <span class="text-danger">*</span>
                  </template>
                  <select v-model="form.selected_provinces" class="form-control">
                    <option
                      v-for="item in provinces"
                      :key="item.id"
                      v-bind:value="item.id"
                    >{{item.name_in_thai}}</option>
                  </select>

                  <b-form-invalid-feedback id="input-last-name-invalid">จังหวัด</b-form-invalid-feedback>
                </b-form-group>

                <!-- อำเภอ -->
                <b-form-group
                  id="group-num-selected_districts"
                  label-for="input-num-selected_districts"
                >
                  <template v-slot:label>
                    อำเภอ
                    <span class="text-danger">*</span>
                  </template>

                  <select v-model="form.selected_districts" class="form-control">
                    <option
                      v-for="item in districts"
                      :key="item.id"
                      v-bind:value="item.id"
                    >{{item.name_in_thai}}</option>
                  </select>

                  <b-form-invalid-feedback id="input-last-name-invalid">อำเภอ</b-form-invalid-feedback>
                </b-form-group>

                <!-- ตำบล -->
                <b-form-group
                  id="group-num-selected_subdistricts"
                  label-for="input-num-selected_subdistricts"
                >
                  <template v-slot:label>
                    ตำบล
                    <span class="text-danger">*</span>
                  </template>

                  <select v-model="form.selected_subdistricts" class="form-control">
                    <option
                      v-for="item in subdistricts"
                      :key="item.id"
                      v-bind:value="item.id"
                    >{{item.name_in_thai}}</option>
                  </select>

                  <b-form-invalid-feedback id="input-last-name-invalid">ตำบล</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-num-postman" label-for="input-num-postman">
                  <template v-slot:label>
                    รหัสไปรษณีย์
                    <span class="text-danger">*</span>
                  </template>

                  <b-form-input
                    id="input-num-postman"
                    type="text"
                    v-model="form.postman"
                    :state="$v.form.postman.$dirty ? !$v.form.postman.$error : null"
                    placeholder="รหัสไปรษณีย์"
                  ></b-form-input>

                  <b-form-invalid-feedback id="input-last-name-invalid">โปรดกรอกรหัสไปรษณีย์</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group id="group-password" label-for="input-password">
                  <template v-slot:label>
                    รหัสผ่าน
                    <span class="text-danger" v-if="formType === 'new'">*</span>
                  </template>

                  <b-form-input
                    id="input-password"
                    type="password"
                    v-model="form.password"
                    :state="$v.form.password.$dirty ? !$v.form.password.$error : null"
                    placeholder="รหัสผ่าน"
                  ></b-form-input>

                  <b-form-invalid-feedback id="input-password-invalid">โปรดกรอกรหัสผ่าน</b-form-invalid-feedback>
                </b-form-group>

                <template v-if="errorMessages">
                  <b-row class="mb-2">
                    <b-col class="text-danger message-col">{{ errorMessages }}</b-col>
                  </b-row>
                </template>

                <b-row>
                  <b-col>
                    <b-button type="submit" size="sm" variant="success" :disabled="loading">
                      <span class="spinner spinner-white" v-if="loading"></span>
                      <font-awesome-icon :icon="['fas', 'save']" class="mr-1" />Save
                    </b-button>
                  </b-col>
                  <b-col class="text-right">
                    <b-button size="sm" variant="warning" href="/login">
                      <font-awesome-icon :icon="['fas', 'long-arrow-alt-left']" class="mr-1" />กลับ
                    </b-button>
                  </b-col>
                </b-row>
              </b-form>
            </div>
          </div>
        </div>
      </div>
      <div class="col-md-3"></div>
    </div>
  </div>
</template>

<script>
import { email, required, minLength, maxLength } from 'vuelidate/lib/validators';
import { mapGetters, mapState } from 'vuex';
import _ from 'lodash';
import moment from 'moment';
import User from '@/model/user';
import utils from '@/helper/utils';
import configService from '@/services/configService';
import axios from 'axios';

export default {
  name: 'UserFormBox',
  props: {
    listUrl: String,
    userType: String,
    formType: String,
    userId: {
      type: Number,
      required: false
    },
    permissions: {
      type: Array,
      required: false
    }
  },
  metaInfo() {
    return {
      meta: [
        {
          name: 'description',
          content: this.metaDescription
        }
      ]
    };
  },
  data() {
    return {
      title: '',
      formLoaded: false,
      form: {
        username: null,
        email: 'test@hotmail.com',
        firstName: null,
        lastName: null,
        nickname: null,
        numreg: null,
        tel: null,
        numhome: null,
        nummoo: null,
        selected_provinces: 0,
        selected_districts: 0,
        selected_subdistricts: 0,
        postman: null,
        password: null,
        confirmedAt: '2020-05-13 13:13:00',
        blockedAt:  null,
        permissions: [3, 2, 4, 1],
        role: 50,
        enabled: 1
      },
      provinces: [{ id: 0, name_in_thai: 'เลือกจังหวัด' }],
      districts: [{ id: 0, name_in_thai: 'เลือกอำเภอ' }],
      subdistricts: [{ id: 0, name_in_thai: 'เลือกตำบล' }],
      dateTimeFormat: configService.get('format').pickerDateTime,
      staffRoles: _.reduce(
        _.pick(User.userRole, ['administrator', 'staff']),
        (result, value, key) => {
          result.push({ value, text: _.capitalize(key) });
          return result;
        },
        []
      ),
      userEnabled: _.reduce(
        User.userEnabled,
        (result, value, key) => {
          result.push({ value, text: _.capitalize(key) });
          return result;
        },
        []
      )
    };
  },
  validations() {
    const formValidation = {
      email: {
        required,
        email
      },
      firstName: {
        required
      },
      lastName: {
        required
      },
      nickname: {
        required
      },
      numhome: {
        required
      },
      nummoo: {
        required
      },
      postman: {
        required
      },
      tel: {
        required,
        minLength: minLength(10),
        maxLength: maxLength(10),
        validateTel: tel => {
          if (tel === null) {
            return false;
          }
          return tel.match(/^[0-9]+$/) !== null;
        }
      },
      numreg: {
        required,
        minLength: minLength(13),
        maxLength: maxLength(13),
        validateNumreg: numreg => {
          if (numreg === null) {
            return false;
          }
          return numreg.match(/^[0-9]+$/) !== null;
        }
      },
      username: {
        required,
        minLength: minLength(3),
        maxLength: maxLength(15),
        validateUsername: username => {
          if (username === null) {
            return false;
          }
          return username.match(/^[0-9a-zA-Z_]+$/) !== null;
        }
      },
      password: {
        minLength: minLength(6)
      },
      confirmedAt: {
        validateDateTime: utils.validateDateTime
      },
      blockedAt: {
        validateDateTime: utils.validateDateTime
      },
      permissions: {},
      role: {
        validateRole: value => {
          if (this.userType === 'user') {
            return true;
          }
          return _.some(User.userRole, role => value === role);
        }
      },
      enabled: {
        validateStatus: value => {
          return _.some(User.userEnabled, enabled => value === enabled);
        }
      }
    };

    if (this.formType === 'new') {
      formValidation.password.required = required;
    }

    return { form: formValidation };
  },
  mounted() {
    this.getProvinces();
    this.$nextTick(async () => {
      // Code that will run only after the entire view has been re-rendered

      this.form.role = User.userRole.staff;

      this.form.enabled = User.userEnabled.active;
      this.formLoaded = true;

      this.$v.$touch(); // Set initial validation
      this.$v.$reset(); // Reset $dirty
    });
  },
  computed: {
    metaDescription() {
      return this.formType === 'new' ? 'Add new user' : 'Update user';
    },
    ...mapGetters('alert', ['errorMessages']),
    ...mapState('user', ['loading', 'user']),
    showPermissions() {
      return this.userType === 'staff' && this.form.role === User.userRole.staff;
    }
  },
  methods: {
    findDataById(arr, id) {
      const index = arr.findIndex(x => x.id === id);
      // console.log("inde " + index);

      return arr[index].name_in_thai;
    },

    getProvinces() {
      axios.get('http://localhost:3000/country/provinces').then(response => {
        this.provinces = this.provinces.concat(response.data.data);
        console.log(response.data.data);
      });
    },
    getDistricts(id) {
      axios.get(`http://localhost:3000/country/districts/${id}`).then(response => {
        this.districts = this.districts.concat(response.data.data);
        // console.log(this.districts);
      });
    },
    getSubdistricts(id) {
      axios.get(`http://localhost:3000/country/subdistricts/${id}`).then(response => {
        // console.log(response);
        this.subdistricts = this.subdistricts.concat(response.data.data);
        // console.log(this.subdistricts);
      });
    },
       postUser(id) {
      axios.post(`http://localhost:3000/staff`,id).then(response => {
          console.log(response);
          
      });
    },
    onSubmit() {
      // this.$v.$touch();
      // if (this.$v.$invalid) {
      //   console.log(this.$v);
      //   console.log(this.form);

      //   return false;
      // }

      const user = {
        username: this.form.username,
        email: this.form.email,
        first_name: this.form.firstName,
        last_name: this.form.lastName,
        nick_name: this.form.nickname,
        numreg: this.form.numreg,
        tel: this.form.tel,
        numhome: this.form.numhome,
        nummoo: this.form.nummoo,
        province: this.form.selected_provinces,
        districts: this.form.selected_districts,
        subdistricts: this.form.selected_subdistricts,
        postman: this.form.postman,
        password: this.form.password,
        confirmed_at: this.form.confirmedAt,
        blocked_at: this.form.blockedAt,
        role: this.form.role ,
        permissions: this.form.permissions,
        enabled: this.form.enabled
      };
     this.postUser(user);
       window.location.reload();
     
        // this.$emit('add', { user });
    
      return false;
    },
    setFormPermissions() {
      if (this.formLoaded) {
        if (this.formType === 'new') {
          _.forEach(this.permissions, (permission, index) => {
            this.form.permissions[index] = true;
          });
        } else {
          _.forEach(this.permissions, (permission, index) => {
            this.form.permissions[index] =
              _.isEmpty(_.find(this.user.permissions, userPermission => userPermission.id === permission.id)) === false;
          });
        }
      }
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
    },
    // 'form.selected_subdistricts': function(val) {},
    user(_newValue, _oldValue) {
      if (!this.user.id) {
        return;
      }
      // Loaded user, assign to form

      this.form.username = this.user.username;
      this.form.email = this.user.email;
      this.form.firstName = this.user.firstName;
      this.form.lastName = this.user.lastName;
      this.form.nickname = this.user.nickname;
      this.form.numreg = this.user.numreg;
      this.form.tel = this.user.tel;
      this.form.numhome = this.user.numhome;
      this.form.nummoo = this.user.nummoo;
      this.form.selected_provinces = this.user.selected_provinces;
      this.form.selected_districts = this.user.selected_districts;
      this.form.selected_subdistricts = this.user.selected_subdistricts;
      this.form.postman = this.user.postman;
      this.form.password = this.user.password;
      this.form.confirmedAt = this.user.confirmedAt;
      this.form.blockedAt = moment(this.user.blockedAt).isValid() ? moment(this.user.blockedAt).toISOString() : null;
      this.form.role = this.user.role;
      this.form.enabled = this.user.enabled;

      this.formLoaded = true;
      this.setFormPermissions();

      this.$v.$touch(); // Set initial validation
      this.$v.$reset(); // Reset $dirty
    },
    permissions(_newValue, _oldValue) {
      this.setFormPermissions();
    }
  }
};
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
