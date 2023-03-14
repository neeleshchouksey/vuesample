<template>
  <div class="">
    <div class="container-fluid">
      <div class="row mob-row">
        <div class="col-md-9">
          <section class="employer-dashboard pink-pattern-bg">
            <h1 class="page-title text-left mb-0">Guided <span class="yellow-bg">Transformation</span></h1>
            <h2 class="page-sub-title">This section will be the opening to the learning journey</h2>
            <!-- <div class="row">
            <div class="col-lg-3 col-md-6 my-2">
              <router-link to="/employer/request-workshop" class="dashboard-card-link">
                Requested Workshops
                <img src="../../../assets/images/chevron-right.svg" alt="arrow-right" />
              </router-link>
            </div>
            <div class="col-lg-3 col-md-6 my-2">
              <router-link to="/employer/workshops" class="dashboard-card-link">
                Workshops
                <img src="../../../assets/images/chevron-right.svg" alt="arrow-right" />
              </router-link>
            </div>
            <div class="col-lg-3 col-md-6 my-2">
              <router-link to="/employer/announcements" class="dashboard-card-link">
                Announcements
                <img src="../../../assets/images/chevron-right.svg" alt="arrow-right" />
              </router-link>
            </div>
            <div class="col-lg-3 col-md-6 my-2">
              <router-link to="/employer/resources" class="dashboard-card-link">
                Resources
                <img src="../../../assets/images/chevron-right.svg" alt="arrow-right" />
              </router-link>
            </div>
          </div> -->
            <div class="row mt-5 card-row emplyr-card-row">
              <div class="col-md-6 mb-3" v-if="stepsList.length" v-for="(step, index) in stepsList"
                v-bind:key="step.id">
                <router-link :to="'/employer/view-step/' + step.id">
                  <div class="card">
                    <img :src="filePath + '/' + step.image" class="card-img-top" alt="...">
                    <div class="card-body">
                      <h6 class="card-sub-title">Step {{ index }}</h6>
                      <h5 class="card-title-2">{{ step.title }}</h5>
                    </div>
                  </div>
                </router-link>
              </div>
              <div v-if="!stepsList.length">No data found</div>
            </div>
          </section>
        </div>
        <div class="col-md-3 bg-light-clr mt-0">
          <RecentSectionVue></RecentSectionVue>
        </div>
      </div>
    </div>
    <section class="employer-dashboard-statistics pink-pattern-bg-2">
      <div class="row">
        <div class="col-md-6 my-3">
          <div class="card-box statistics-box">
            <h5>Resources</h5>
            <router-link to="/employer/resources" class="pera" v-for="rl in resourcesList" v-bind:key="rl.id">{{ rl.title }} <img
                src="../../../assets/images/chevron-right-blue.svg" /></router-link>
            <p v-if="!resourcesList.length">No Data Found</p>
            <router-link to="/employer/resources">View All</router-link>
          </div>
        </div>
        <div class="col-md-6 my-3">
          <div class="card-box statistics-box">
            <h5>"To Do"</h5>
            <div class="todo-map-list" v-if="todoList.length">
              <div v-for="todo in todoList" v-bind:key="todo.id">
                <img src="../../../assets/images/todo-map-icon.svg" />
                <h4>{{todo.title | truncate(4)}}</h4>
                <p>{{ todo.description | truncate(10) }}</p>
              </div>
            </div>
            <p v-if="!todoList.length">No Data Found</p>
            <router-link to="/employer/todo">View All</router-link>
          </div>
        </div>
        <div class="col-md-6 my-3">
          <div class="card-box statistics-box">
            <h5>Workshops</h5>
            <p v-if="!workshopsListDashboard.length">No Data Found</p>
            <router-link v-if="workshopsListDashboard.length" class="pera" v-for="rl in workshopsListDashboard"
              v-bind:key="rl.id" :to="'/employer/view-workshop/' + rl.id">{{ rl.title }} <img
                src="../../../assets/images/chevron-right-blue.svg" /></router-link>
            <router-link to="/employer/workshops">View All</router-link>
          </div>
        </div>
        <div class="col-md-6 my-3">
          <div class="card-box statistics-box">
            <h5>Opportunities</h5>
            <p v-if="opprotunity.content" v-html="opprotunity.content"></p>
            <p v-if="!opprotunity.content">No Data Found</p>
            <router-link to="/employer/opportunities">View All</router-link>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import RequestWorkshop from '../../admin/RequestWorkshop.vue'
import RecentSectionVue from '../../common/RecentSection.vue'

export default {
  components: {   RequestWorkshop, RecentSectionVue },
  name: 'Dashboard',
  mixins: [AppMixin],
  data() {
    return {
      resourcesList: [],
      announcementsList: [],
      stepsList: [],
      filePath: '',
      opprotunity: {
        content: ''
      }
    }
  },
  methods: {
    getOpportunityListDashboard: function () {
      let that = this
      Api.getOpportunityListDashboard(that.company.id).then(response => {
        let that = this
        that.opprotunity.content = response.data.res.content
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
    getAnnouncementsList: function () {
      let that = this
      Api.getAnnouncementsList(that.company.id).then(response => {
        let that = this
        that.announcementsList = response.data.res
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
    getStepsList: function () {
      let that = this
      Api.getStepsList(1, {}).then(response => {
        let that = this
        that.stepsList = response.data.res
        that.filePath = response.data.path
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
    getResourcesListDashboard: function () {
      let that = this
      Api.getResourcesListDashboard().then(response => {
        let that = this
        that.resourcesList = response.data.res
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
  },
  created() {
    this.getAnnouncementsList()
    this.getStepsList()
    this.getResourcesListDashboard()
    this.getOpportunityListDashboard()
    this.getTodoListDashboard()
    this.getRequestedWorkshopListDashboard()
    this.getWorkshopsListDashboard()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
