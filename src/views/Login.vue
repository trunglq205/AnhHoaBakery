<template>
  <div class="banner_account">
    <div>
      <div class="overlay_account"></div>
      <v-main>
        <div class="title_account">
          <p><b>TÀI KHOẢN</b></p>
        </div>
        <div class="form_account">
          <v-row class="inner">
            <v-col cols="4" offset="4">
              <v-form @submit.prevent="HandleSubmit">
                <h4><b>Đăng Nhập</b></h4>
                <hr />
                <error v-if="error" :error="error" />
                <p>
                  <input type="email" placeholder="Email" v-model="email" />
                </p>
                <p>
                  <input
                    type="password"
                    placeholder="Mật khẩu"
                    v-model="password"
                  />
                </p>
                <p><button>Đăng nhập</button></p>
                <router-link to="/"><p>Trở về</p></router-link>
                <router-link to="/account/register"><p>Đăng kí</p></router-link>
                <router-link to=""><p>Quên mật khẩu?</p></router-link>
              </v-form>
            </v-col>
          </v-row>
        </div>
      </v-main>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import Error from "../components/Error.vue";
export default {
  name: "Login",
  components: {
    Error,
  },
  data: () => ({
    email: "",
    password: "",
    error: "",
    user: {
      firstName: "",
      lastName: "",
      email: "",
      id: ""
    }
  }),
  methods: {
    async HandleSubmit() {
      try {
        const response = await axios.post("/user/login",
        {
          email: this.email,
          password: this.password,
        });
        if(this.email == "admin@anhhoa.com"){
          localStorage.setItem("Admin", JSON.stringify(response.data[0]._id));
          this.$router.push("/admin");
        }
        else{
          this.user.firstName = response.data[0].fistName;
          this.user.lastName = response.data[0].lastName;
          this.user.email = response.data[0].email;
          this.user.id = response.data[0]._id;
          localStorage.setItem("User", JSON.stringify(this.user));
          this.$router.push("/account");
        }
      } catch (e) {
        this.error = "Email/Mật khẩu không hợp lệ!";
      }
    },
  },
};
</script>

<style scoped>
.banner_account {
  background: url(//theme.hstatic.net/1000313040/1000406925/14/breadcrumb_bg.png?v=1757);
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
  overflow: hidden;
}
.overlay_account {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 0;
  background: rgba(0, 0, 0, 0.5);
}
.title_account {
  text-align: center;
  color: white;
  padding: 120px 0px;
  font-size: 35px;
}
.form_account {
  background: #f8f2e8;
}
.inner {
  padding: 50px 180px;
}
.v-form h4 {
  font-size: 25px;
  text-align: center;
}
.v-form input {
  width: 100%;
  border: 1px solid #e6e6e6;
  max-width: 100%;
  padding: 8px 10px;
  border-radius: 3px;
  background: white;
}
.v-form button {
  padding: 8px 10px;
  font-weight: bold;
  border-radius: 3px;
  background-color: #3d1a1a;
  color: #fff;
  border: 1px solid transparent;
  width: 100%;
}
.v-form a {
  color: #333333;
  text-align: center;
}
</style>