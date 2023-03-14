<template>
  <div>
    <section class="team-management-section half-cut-bg">
      <h1 class="page-title text-left">
        Customer <span>Support</span>
      </h1>
      <div class="card-box">
        <div class="row">
          <div class="col-md-6">
            <div class="card">
              <div class="card-body">
                <img src="../../assets/images/registration.svg" class="mb-4" alt="Registration" />
                <h5 class="card-title">Ask Question</h5>
                <!-- <p class="card-text">Manage your employee list (add or delete team members). You can also monitor login activity.</p> -->
                <router-link to="/employee/ask-question" class="links">Visit Link</router-link>
              </div>
            </div>
          </div>
          <div class="col-md-6 mb-3">
            <div class="card">
              <div class="card-body">
                <img src="../../assets/images/registration.svg" class="mb-4" alt="Registration" />
                <h5 class="card-title">Support Requests</h5>
                <!-- <p class="card-text">Use this link to send an invite to your team in order to expedite platform registration.</p> -->
                <router-link to="/employee/ask-your-care-team" class="links">Visit Link</router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
/* eslint-disable */

export default {
  name: 'CustomerSupport',
  data() {
    return {
    }
  },
  methods: {

  },
  mounted() {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
