<template>
  <div class="login-page loginDialog">
    <div class="add-client ">
      <div class="overlay"></div>
      <v-alert
        type="info"
        :color="colorAlert"
        v-if="showMsg"
        border="bottom"
        colored-border
        transition="leave-to-class"
        elevation="2"
        dismissible
      >
        {{ msg }}
      </v-alert>
      <v-col cols="4">
        <v-btn
          color="info darken-1"
          class="d-flex justify-start"
          height="50"
          @click="restoreBCApi"
          >استعادة نسخة احتياطية</v-btn
        >
      </v-col>
      <!-- <v-card-title>
                  <span class="headline title"> نماء للبذور</span>
                </v-card-title> -->
      <v-card class="text-center login">
        <v-card-title>
          <p class="headline">
            <v-icon color="#fff" large>
              mdi-account-key
            </v-icon>
            تسجيل دخول
          </p>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row class="text-center">
              <v-col cols="12">
                <v-text-field
                  label="الرقم السري"
                  v-model="password"
                  prepend-inner-icon="mdi-account-key"
                  outlined
                  type="password"
                  v-on:keyup.enter="login"
                  dark
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions class="justify-center ">
          <v-btn
            color="green   darken-1"
            min-width="130"
            height="50"
            @click="login"
            >تسجيل الدخول</v-btn
          >
        </v-card-actions>
      </v-card>
      <v-footer absolute padless>
        <v-col class="text-center" cols="12">
          <strong
            >هذه نسخة خاصة للمهندس مصطفي ابراهيم
            <span class="info--text mr-4">
              {{ new Date().getFullYear() }} —جميع الحقوق محفوظة</span
            >
            ارقام التواصل<span> 01069005019 — 01144624418 </span>
          </strong>
        </v-col>
      </v-footer>
    </div>
  </div>
</template>

<script>
import Axios from "axios";
export default {
  data: () => ({
    colorAlert: "red",
    showMsg: false,
    msg: "",
    passData:null,
    password: "",
    restoreBC: "",
    authLogin: true,
    loginApi: null,
  }),
  created() {
    setInterval(() => {
      this.showMsgSuc = false;
      this.showMsg = false;
    }, 5000);
    if (sessionStorage.getItem("password") !== null || undefined) {
      // this.password == sessionStorage.getItem("password");
      this.$store.dispatch("autoLogin");
    }
  },
  watch: {
    user(value) {
      if (value !== null && value !== undefined) {
        this.$router.push("/");
      }
    },
  },
  mounted() {},
  computed: {
    user() {
      return this.$store.getters.user;
    },
    error() {
      return this.$store.getters.error;
    },
  },
  methods: {
    alertMsg(bool, color, msgs) {
      this.showMsg = bool;

      this.colorAlert = color;
      this.msg = msgs;
    },
    login() {
      if (this.password !== "") {
        this.$store.dispatch("loginVuex", {
          password: this.password,
        });
      } else {
        this.alertMsg(true,'red'," ادخل كلمة المرور من فضلك");
      }
    },
    restoreBCApi() {
      var validate = prompt("ادخل كلمة السر لإعادة البيانات");
      Axios.get(`http://localhost:8087/api/restoreBackup/${validate}`)
        .then((res) => {
          this.passData = res.data;
          if (res.status == 200) {
            this.alertMsg(true,'green',"تم الاستعادة");
          }
        })
        .catch((err) => {
          var Console = console;
          Console.log(err);
        });
    },
  }
};
</script>
