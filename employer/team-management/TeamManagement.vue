<template>
  <div>
    <section class="team-management-section half-cut-bg">
      <h1 class="page-title text-left">
        Team and Organization <span>Management</span>
      </h1>
      <div class="card-box">
        <div class="row">
           <div class="col-md-6">
            <div class="card">
              <div class="card-body">
                <img src="../../../assets/images/registration.svg" class="mb-4" alt="Registration" />
                <h5 class="card-title">Employees</h5>
                <p class="card-text">Manage your employee list (add or delete team members). You can also monitor login activity.</p>
                <router-link to="/employer/team-management/employees" class="links">Visit Link</router-link>
              </div>
            </div>
          </div>
          <div class="col-md-6 mb-3">
            <div class="card">
              <div class="card-body">
                <img src="../../../assets/images/registration.svg" class="mb-4" alt="Registration" />
                <h5 class="card-title">Registration</h5>
                <p class="card-text">Use this link to send an invite to your team in order to expedite platform registration.</p>
                <router-link to="/employer/team-management/registration-link" class="links">Visit Link</router-link>
              </div>
            </div>
          </div>
          <div class="col-md-6 mb-3">
            <div class="card">
              <div class="card-body">
                <img src="../../../assets/images/registration.svg" class="mb-4" alt="Registration" />
                <h5 class="card-title">Invitations</h5>
                <p class="card-text">If you have already sent a link to team members or added a new employee, use this link to monitor outstanding invites to the platform.</p>
                <router-link to="/employer/team-management/invitations" class="links">Visit Link</router-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
<!--     <h1>Team and Organization Management</h1>
    <div class="row col-md-12">
      <div class="col-md-3">
        <router-link to="/employer/team-management/registration-link">
          <div class="icons">
            <i class="fa fa-link"></i>
            <span>Registration Link</span>
          </div>
        </router-link>
        <p>Use this link to send an invite to your team in order to expedite platform registration.</p>
      </div>
      <div class="col-md-3">
        <router-link to="/employer/team-management/employees">
          <div class="icons">
            <i class="fa fa-users"></i>
            <span>Employees</span>
          </div>
        </router-link>
        <p>
          Manage your employee list (add or delete team members). You can also monitor login activity.
        </p>
      </div>
      <div class="col-md-3">
        <router-link to="/employer/team-management/invitations">
          <div class="icons">
            <i class="fas fa-user-plus"></i>
            <span>Invitations</span>
          </div>
        </router-link>
        <p>If you have already sent a link to team members or added a new employee, use this link to monitor
outstanding invites to the platform.</p>
      </div>
       <div class="col-md-3">
        <div class="icons">
          <i class="fa fa-users"></i>
          <span>Employees</span>
        </div>
      </div>
      </div> -->
  </div>
</template>
<script>
/* eslint-disable */

export default {
  name: 'TeamManagement',
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
