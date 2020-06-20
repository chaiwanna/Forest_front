<template>
  <div class="page-class page-login">
    <b-container class=" my-5">
      <b-row align-v="center" align-h="center" align-content="center">
        <b-col class="text-center" sm="10" md="8" lg="5">
          <h1 class="name-system">ระบบลงทะเบียนผู้เข้าใช้พื้นที่เขตรักษาพันธุ์สัตว์ป่าเวียงลอ</h1>

          <b-card-group deck class="mt-3">
            <b-card>
              <h1>Login</h1>
              <p class="text-muted"></p>
              <login-box />
            </b-card>
          </b-card-group>
          <br />
          &copy; {{ thisYear }}
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { mapActions } from 'vuex';
import LoginBox from '@/components/LoginBox.vue';

export default {
  name: 'Login',
  components: { LoginBox },
  metaInfo: {
    title: 'Login',
    meta: [
      {
        name: 'description',
        content: 'Login backend with your account.'
      }
    ]
  },
  mounted() {
    if (this.$route.query.messageKey) {
      this.handleAuthMessageKey({ messageKey: this.$route.query.messageKey });
    }
  },
  methods: {
    ...mapActions('auth', ['handleAuthMessageKey'])
  },
  computed: {
    thisYear() {
      return new Date().getFullYear();
    }
  }
};
</script>
<style scoped>
.name-system {
  width: 80%;
  display: block;
  margin: auto;
  margin-bottom: 40px;
}
h1 {
  font-size: 18px;
  font-weight: 600;
  text-align: center;
}
</style>