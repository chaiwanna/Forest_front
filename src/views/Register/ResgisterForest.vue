<template>
  <div class="register container">
    <div class="row">
      <div class="col-md-3"></div>
      <div class="col-md-6">
        <h1 class="name-system">ระบบลงทะเบียนผู้เข้าใช้พื้นที่เขตรักษาพันธุ์สัตว์ป่าเวียงลอ</h1>
        <div class="card">
          <h1 class="card-header">แจ้งการเข้าใช้พื้นที่ป่าไม้</h1>
          <div class="card-body">
            <div v-if="loading || !formLoaded">
              <span class="spinner"></span>
            </div>
            <div v-if="!loading && formLoaded">
              <b-form @submit.stop.prevent="onSubmit">
                <!-- ตำบล -->
                <b-form-group id="group-num-selected_zone" label-for="input-num-selected_zone">
                  <template v-slot:label>
                    โซนบริเวณป่าไม้ที่จะเข้า
                    <span class="text-danger">*</span>
                  </template>

                  <select v-model="form.selected_zone" class="form-control">
                    <option v-for="item in zone" :key="item.name" v-bind:value="item.name">{{item.name}}</option>
                  </select>

                  <b-form-invalid-feedback id="input-selected_zone-invalid">โซนบริเวณป่าไม้ที่จะเข้า</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group label="วัตถุประสงค์ในการเข้าป่า">
                  <b-form-checkbox-group id="checkbox-group-2" v-model="form.objective" name="flavour-2">
                    <b-form-checkbox value="ผักหวาน">ผักหวาน</b-form-checkbox>
                    <b-form-checkbox value="ไข่มดแดง">ไข่มดแดง</b-form-checkbox>
                    <b-form-checkbox value="หน่อไม้">หน่อไม้</b-form-checkbox>
                    <b-form-checkbox value="เห็ดดิน">เห็ดดิน</b-form-checkbox>
                    <b-form-checkbox value="เห็ดลม">เห็ดลม</b-form-checkbox>
                    <br />
                    <b-form-checkbox value="เห็ดกระด้าง">เห็ดกระด้าง</b-form-checkbox>
                    <b-form-checkbox value="เห็ดถอบ">เห็ดถอบ</b-form-checkbox>
                    <b-form-checkbox value="สมุนไพร">สมุนไพร</b-form-checkbox>
                    <b-form-checkbox value="ผลไม้ป่า">ผลไม้ป่า</b-form-checkbox>
                    <b-form-checkbox value="ไม้ฟืน">ไม้ฟืน</b-form-checkbox>
                    <br />
                    <b-form-checkbox value="ยอดผัก">ยอดผัก</b-form-checkbox>
                    <b-form-checkbox value="ใบไม้">ใบไม้</b-form-checkbox>
                    <b-form-checkbox value="ไม้ไผ่">ไม้ไผ่</b-form-checkbox>
                  </b-form-checkbox-group>
                </b-form-group>
                <div>
                  Selected:
                  <strong>{{ form.objective }}</strong>
                </div>

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
                    <b-button size="sm" variant="warning" href="/">
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
import User from '@/model/user';
import utils from '@/helper/utils';
import axios from 'axios';

export default {
  name: 'resgisterforest',
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
        selected_zone: 0,
        other: null,
        objective:[]
      },
      zone: [{ id: 0, name: 'เลือกโซนบริเวณป่าไม้ที่จะเข้า' }]
    };
  },
  validations() {
    const formValidation = {
      email: {
        required,
        email
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
    this.getZone();
    this.getUser(this.user.id);
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
    ...mapState('auth', ['user']),
    showPermissions() {
      return this.userType === 'staff' && this.form.role === User.userRole.staff;
    }
  },
  methods: {
    findDataById(arr, id) {
      const index = arr.findIndex(x => x.id === id);
      // console.log("inde " + index);

      return arr[index].NAME;
    },
     getUser(id) {
      axios.get(`http://localhost:3000/staff/${id}`).then(response => { 
        console.log(response.data);
      });
    },

    getZone() {
      axios.get('http://localhost:3000/getregisterforest').then(response => {
        this.zone = this.zone.concat(response.data.data);
        console.log(response.data.data);
      });
    },

    postforest(id) {
      axios.post('http://localhost:3000/getregisterforest',id).then(response => {  
        console.log(response.data.data);
      });
    },
  onSubmit() {
    const objectiveArr =this.form.objective;
    const objectiveString = objectiveArr.toString();
      const userforest = {
        USER_IDUSER : this.user.id,
        FOREST_DETAIL_ID: this.form.selected_zone,
        OBJECTIVE: objectiveString
      };
      console.log(userforest);
      
      this.postforest(userforest);
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

    // 'form.selected_subdistricts': function(val) {},
    user(_newValue, _oldValue) {
      if (!this.user.id) {
        return;
      }
      // Loaded user, assign to form

      this.form.username = this.user.username;
      this.form.email = this.user.email;
      this.form.firstName = this.user.firstName;

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
