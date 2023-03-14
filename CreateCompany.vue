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
            <p class="semi-bold text-gray f-18">Company Creation</p>
            <Add :modal=0></Add>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
// import Api from '../router/api'
/* eslint-disable */
import Api from '../router/api'
import Add from '../components/common/companies/Add.vue'
export default {
  name: 'CreateCompany',
  data() {
    return {
      msg: 'Create Company',

    }
  },
  components: { Add },
  methods: {


  },
  mounted() {
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
