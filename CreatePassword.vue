<template>
  <section class="login-section">
    <div class="login-inner">
      <div class="login-box">
        <div class="login-left">
          <div class="login-bg-text">
            <div class="left-content">
              <h1>Mpact International</h1>
              <h5>Innovating Change,</h5>
              <h5>One Perspective at a Time </h5>
            </div>
          </div>
        </div>
        <div class="login-right">
          <div class="login-form-box">
            <div class="logo">
              <a href="javascript:void(0)">
                <img src="../assets/images/logo.png">
              </a>
            </div>
            <p class="semi-bold text-gray f-18">Create Password Here</p>
            <form class="login-from">
              <div class="form-group">
                <input type="password" class="form-control" id="password" placeholder="Enter New Password"
                  v-model="resetPass.password">
              </div>
              <div class="form-group">
                <input type="password" class="form-control" id="cpassword" placeholder="Retype New Password"
                  v-model="resetPass.cpassword">
              </div>
              <button type="button" class="d-block btn btn-primary" @click="resetPassword"
                :disabled="resetPass.disabled">Create Password</button>
              <!-- <p class="forget-link text-center mt-3"><a href="#" class="text-red">Login</a></p> -->
            </form>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Api from '../router/api'
/* eslint-disable */
export default {
  name: 'ResetPassword',
  data() {
    return {
      msg: 'Reset Password',
      resetPass: {
        'password': '',
        'cpassword': '',
        'disabled': false,
      }
    }
  },
  methods: {
    resetPassword: function () {
      let that = this;
      if (!that.resetPass.password || !that.resetPass.cpassword) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else if (that.resetPass.password != that.resetPass.cpassword) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Password and Confirm Password not matched",
          showConfirmButton: true
        });
      }
      else {
        that.resetPass.disabled = true;
        that.resetPass.link = this.$route.params.link;

        let url = this.$route.fullPath.split('/')[1]

        Api.resetPassword(that.resetPass, url).then(response => {
          that.resetPass.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Password Created Succcessfully",
            showConfirmButton: true
          }).then(function () {
            that.resetPass.disabled = false;
            that.$router.push('/login');
          });

        }
        ).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.resetPass.disabled = false;
          });
        });
      }
    },
  },
  mounted() {
    let url = this.$route.fullPath.split('/')[1]
    console.log(url)
    if (url == 'create-password') {
      this.msg = 'Create Password'
    } else {
      this.msg = 'Reset Password'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form-group {
  margin-bottom: 1rem;
}

.err {
  color: red;
}
</style>
