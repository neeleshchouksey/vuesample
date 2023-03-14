<template>
  <div class="">
    <section class="my-learning-plan">
      <h2 class="page-title text-left mb-0  d-inline-block">My Learning <span>Plan</span></h2>
      <h4 class="page-sub-title">This section will be the opening to the learning journey</h4>
      <div class="row" v-if="learningPlanLength">
        <div class="col-md-6 col-lg-4 mb-3" v-for="img in learningPlan.data" v-bind:key="img.id">
          <div class="card">
            <img :src="learningPlanPath + '/' + img.image"  class="card-img-top"
                 alt="...">
            <div class="card-body">
              <h5 class="card-title">{{ img.title | truncate(15) }}</h5>
              <p class="card-text">{{ img.description | truncate(100) }}</p>
              <router-link class="links" :to="'/'+currentUrl+'/my-learning-plan/'+img.id">
                Read More <img src="../../assets/images/arrow-right.svg"/>
              </router-link>
            </div>
          </div>
        </div>
      </div>
      <div class="row" v-if="!learningPlanLength">
        No data found
      </div>
      <pagination :data="learningPlan" @pagination-change-page="getLearningPlanList"/>
    </section>
  </div>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'

export default {
  name: 'WelcomeNote',
  mixins: [AppMixin],
  data () {
    return {
      note: {}
    }
  },
  methods: {},
  created () {
    var url = document.URL.split('/');
        this.currentUrl = url[3]
    this.getLearningPlanList()
  }
}
</script>
