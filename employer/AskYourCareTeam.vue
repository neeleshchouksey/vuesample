<template>
  <section class="employer-ask-question-section half-cut-bg">
    <router-link to="/employer/customer-support" class="btn back">
        <!-- <button class="btn-primary"> -->
        <img src="../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
        <!-- </button> -->
      </router-link>
    <h1 class="page-title text-left">Support <span>Requests</span></h1>
    <AskYourCareTeamEmployer></AskYourCareTeamEmployer>
</section>
</template>

<script>
/* eslint-disable */
import AskYourCareTeamEmployer from '../common/customer-support/AskYourCareTeam.vue'
export default {
name: 'AskYourCareTeam',
components: { AskYourCareTeamEmployer },
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
