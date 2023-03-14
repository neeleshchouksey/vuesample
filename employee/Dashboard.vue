<template>
  <div class="container-fluid">
    <div class="row mob-row">
      <div class="col-md-9">
        <section class="employee-dashboard-hero-section" v-if="section1.id">
          <h2 class="page-title mb-0 text-left  d-inline-block">
            {{ section1.title }}
            <!--        <span class="yello-color">Program</span>-->
          </h2>
          <h4 class="page-sub-title" v-html="section1.description"></h4>
          <!--      <img src="../../assets/images/about-frame.svg" class="hero-img" alt="">-->
          <div class="color-border img-max-width">
            <img :src="section1.image" class="hero-img card-img-top" alt="" />
          </div>
        </section>
      </div>
      <div class="col-md-3 bg-light-clr">
       <RecentSectionVue></RecentSectionVue>
      </div>
    </div>
    <section class="employee-dashboard-section" v-if="section2.id">
      <h2 class="page-title mb-0 text-left  d-inline-block">
        {{ section2.title }}
        <!--        This <span>section</span>text-->
      </h2>
      <h4 class="page-sub-title" v-html="section2.description"></h4>
    </section>
    <section class="employee-dashboard-popup-survey">
      <h2 class="page-title mb-0 text-left  d-inline-block">
        Popup <span>Survey</span>
      </h2>
      <h4 class="page-sub-title">
        This section will be the opening to the learning journey
      </h4>
      <div class="popup-survey-box col-md-12 col-lg-10">
        <CustomSurvey v-if="pg.length" :surveyProp="pg" :submitPopupSurvey="submitPopupSurvey"></CustomSurvey>
        <BarChart v-else :chartData="res" :question="chartData.question"></BarChart>
      </div>
    </section>
    <section class="my-learning-plan home-learning-bg" v-if="section3.id">
      <h2 class="page-title text-left mb-0  d-inline-block">
        {{ section3.title }}
        <!--        <span class="yello-color">About Self</span>-->
      </h2>
      <h4 class="page-sub-title" v-html="section3.description"></h4>
      <div class="row">
        <div class="col-md-6 col-lg-4 mb-3" v-for="img in learningPlan" v-bind:key="img.id">
          <div class="card">
            <img :src="learningPlanPath + '/' + img.image" class="card-img-top" alt="..." />
            <div class="card-body">
              <h5 class="card-title">{{ img.title | truncate(15) }}</h5>
              <p class="card-text">{{ img.description | truncate(50) }}</p>
              <router-link class="links" :to="'/'+currentUrl+'/my-learning-plan/' + img.id">
                <!--              <a href="#" class="links">-->
                Read More <img src="../../assets/images/arrow-right.svg" />
                <!--              </a>-->
              </router-link>
            </div>
          </div>
        </div>
        <div class="col-md-12">
          <router-link :to="'/'+currentUrl+'/my-learning-plan'" class="btn my-4">See More</router-link>
        </div>
      </div>
    </section>
    <section class="upcoming-workshops">
      <h2 class="page-title text-left   d-inline-block">
        Upcoming <span class="white-color">Workshops</span>
      </h2>
      <div class="workshops-slider">
        <!-- Swiper -->
        <div>
          <VueSlickCarousel v-if="workshopsListDashboard.length" :arrows="true" :dots="true">
            <div v-for="wl in workshopsListDashboard" v-bind:key="wl.id">
              <div class="row align-items-center">
                <div class="col-md-6">
                  <img :src="wl.image" class="w-100 workshops-img card-img-top" alt="workshops image" />
                </div>
                <div class="col-md-6 px-4">
                  <h3 class="section-title mt-4 mb-2">{{ wl.title }}</h3>

                  <div class="d-flex flex-wrap">
                    <span class="mb-2">
                      <img src="../../assets/images/clock.svg" class="icon" alt="workshops image" />
                      Time: {{ wl.total_hours }} hr(s)</span>
                    <span class="mb-2">
                      <img src="../../assets/images/avtar.svg" class="icon" alt="workshops image" />
                      Leader: {{ wl.instructor }}</span>
                    <span class="mb-2"><img src="../../assets/images/clock.svg" class="icon" alt="workshops image" />
                      Date: {{ wl.date | timeStampToDate }}</span>
                  </div>
                  <button v-if="!wl.registered" class="btn gradient-color-btn mt-4 mb-5"
                    @click="registerForWorkshop(wl.id)">
                    Register Now
                  </button>
                  <button v-if="wl.registered" class="btn gradient-color-btn mt-4 mb-0">
                    Registered
                  </button>
                </div>
              </div>
            </div>
          </VueSlickCarousel>
          <div v-if="!workshopsListDashboard.length">
            No data found
          </div>
        </div>
      </div>
    </section>
    <section class="steps-to-help">
      <h2 class="page-title text-left   d-inline-block">"To Do"</h2>
      <div v-if="todoList.length" class="steps-box" v-for="(todo, index) in todoList" v-bind:key="todo.id">
        <div class="steps-count">
          {{ todo.title | truncate(4) }} <span>{{ index + 1 }}</span>
        </div>
        <div class="pera">
          <p>{{ todo.description }}</p>
        </div>
      </div>
      <div v-if="!todoList.length">
        No data found
      </div>
      <router-link :to="'/'+currentUrl+'/todo'" class="btn mt-4 mb-5">View All</router-link>
    </section>

  </div>
</template>
<script>
/* eslint-disable */
import AppMixin from "../../mixins/AppMixin";
import Api from "../../router/api";
import CustomSurvey from "./CustomSurvey.vue";
import BarChart from "../common/BarChart.vue";
import VueSlickCarousel from "vue-slick-carousel";
import "vue-slick-carousel/dist/vue-slick-carousel.css";
// optional style for arrows & dots
import "vue-slick-carousel/dist/vue-slick-carousel-theme.css";
import RecentSectionVue from "../common/RecentSection.vue";

export default {
  name: "Dashboard",
  mixins: [AppMixin],
  components: {
    CustomSurvey,
    BarChart,
    VueSlickCarousel,
    RecentSectionVue
  },
  data() {
    return {
      pg: [],
      learningPlan: [],
      learningPlanPath: "",
      chartData: [],
      chartDataPer: [],
      res: [],
      currentUrl:''
    };
  },
  methods: {
    registerForWorkshop: function (id) {
      let that = this;
      Api.registerForWorkshop(id).then(response => {
        this.$swal({
          icon: "success",
          title: "Success",
          text: "You have successfully registered for workshop",
          showConfirmButton: true
        });
        this.getWorkshopsListDashboard();
      });
    },
    getSurveyQuestionsDashboard: function () {
      let that = this;
      Api.getSurveyQuestionsDashboard().then(response => {
        this.pg = response.data.res;
        that.getChartData();
      });
    },
    submitPopupSurvey: function (survey) {
      let that = this;
      let data = JSON.stringify(survey.data, null, 3);
      this.surveyRes = JSON.parse(data);
      Api.submitPopupSurvey(this.surveyRes).then(response => {
        that.getSurveyQuestionsDashboard();
      });
    },
    getLearningPlanListDashboard: function () {
      let that = this;
      Api.getLearningPlanListDashboard()
        .then(response => {
          that.learningPlan = response.data.res;
          that.learningPlanPath = response.data.path;
        })
        .catch(error => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () { });
        });
    },
    getChartData: function () {
      let that = this;
      Api.getChartData()
        .then(response => {
          that.chartData = response.data.res;
          that.chartDataPer = response.data.per;
          while (that.res.length > 0) {
            that.res.pop();
          }
          that.res.push(
            [this.chartData.option_1, this.chartDataPer.per1],
            [this.chartData.option_2, this.chartDataPer.per2],
            [this.chartData.option_3, this.chartDataPer.per3],
            [this.chartData.option_4, this.chartDataPer.per4]
          );
        })
        .catch(error => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () { });
        });
    }
  },
  created() {
    var url = document.URL.split('/');
        this.currentUrl = url[3]
    this.getTodoListDashboard();
    this.getRequestedWorkshopListDashboard();
    this.getSection1(this.company.profile_type_id);
    this.getSection2(this.company.profile_type_id);
    this.getSection3(this.company.profile_type_id);
    this.getSurveyQuestionsDashboard();
    this.getWorkshopsListDashboard();
    this.getLearningPlanListDashboard();
  }
};
// var swiper = new Swiper(".mySwiper", {});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.sd-page {
  padding: 0 !important;
}

.sd-root-modern {
  background: transparent !important;
}
</style>
