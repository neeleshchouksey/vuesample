<template>
  <div class="col-md-9 ml-4">
    <div class="row mt-3">
      <CustomSurvey v-if="pg.length" :surveyProp="pg" :submitPopupSurvey="submitPopupSurvey"></CustomSurvey>
    </div>
    <div class="row mt-3">
      <div class="col-md-8">
        <h1><b>{{ section1.title }}</b></h1>
        <p>{{ section1.description }}</p>
      </div>
      <div class="col-md-4">
        <img :src="section1.image" />
      </div>
      <div class="row mt-3">
        <div class="col-md-12">
          <h2><b>{{ section2.title }}</b></h2>
          <p>{{ section2.description }}</p>
        </div>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col-md-6">
        <h1><b>{{ section3.title }}</b></h1>
        <p>
          {{ section3.description }}
        </p>
        <button class="logout-btn btn">Continue Learning</button>
      </div>
      <div class="col-md-6">
        <div class="row">
          <div class="col-md-4 mt-4" v-for="img in section3.image" v-bind:key="img.id">
            <img :src="section3.path + '/' + img.image" />
          </div>
        </div>
      </div>
    </div>
    <div class="row mt-5">
      <div class="col-lg-6 col-md-6">
        <ul class="work-progress-list">
          <p>
            <b class="mb-5">Upcoming Workshops</b>
            <router-link to="/employee/request-workshop">View All</router-link>
          </p>
          <li v-if="workshopList.length" v-for="wl in workshopList" v-bind:key="wl.id">
            <h5 class="text-blue">{{ wl.name }}</h5>
          </li>
          <p v-if="!workshopList.length">No Data Found</p>
        </ul>
      </div>
      <div class="col-lg-6 col-md-6">
        <div class="resource-box">
          <h2 class="text-blue bold mb-3"><span class="primary-text rotated">"</span>To Do<span
              class="primary-text rotated">"</span></h2>
          <router-link to="/employee/todo">View All</router-link>
          <ul class="to-do-list" v-if="todoList.length">
            <li v-for="todo in todoList" v-bind:key="todo.id">
              <router-link :to="'/employee/todo/' + todo.id">Item #{{ todo.id }}</router-link>
              <p class="text-blue op-7">{{ todo.title }}</p>
            </li>
          </ul>
          <p v-if="!todoList.length">
            No Data Found
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import CustomSurvey from './CustomSurvey.vue'

export default {
  name: "Dashboard",
  mixins: [AppMixin],
  components: { CustomSurvey },
  data() {
    return {
      pg: [],
    }
  },
  methods: {
    getSurveyQuestionsDashboard: function () {
      Api.getSurveyQuestionsDashboard().then(response => {
        this.pg = response.data.res;
      });
    },
    submitPopupSurvey: function (survey) {
      let data = JSON.stringify(survey.data, null, 3)
      this.surveyRes = JSON.parse(data)
      Api.submitPopupSurvey(this.surveyRes).then(response => {

      });
    }
  },
  created() {
    this.getTodoListDashboard();
    this.getRequestedWorkshopListDashboard();
    this.getSection1(this.company.profile_type_id);
    this.getSection2(this.company.profile_type_id);
    this.getSection3(this.company.profile_type_id);
    this.getSurveyQuestionsDashboard();
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
