<template>
    <section class="employer-ask-question-section half-cut-bg">
      <router-link to="/employer/customer-support" class="btn back">
        <!-- <button class="btn-primary"> -->
        <img src="../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
        <!-- </button> -->
      </router-link>
      <h1 class="page-title text-left">Customer <span>Support</span></h1>
      <AskQuestionEmployer></AskQuestionEmployer>
  </section>
</template>

<script>
/* eslint-disable */
import AskQuestionEmployer from '../common/customer-support/AskQuestion.vue'

export default {
  name: 'AskQuestion',
  components: { AskQuestionEmployer },
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
