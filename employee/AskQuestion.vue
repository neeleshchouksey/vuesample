<template>
  <section class="employee-ask-question-section half-cut-bg">
    <router-link to="/employee/customer-support" class="btn back">
        <!-- <button class="btn-primary"> -->
        <img src="../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
        <!-- </button> -->
      </router-link>
      <h1 class="page-title text-left">Ask Your <span>Care Team</span></h1>
      <AskQuestionEmployee></AskQuestionEmployee>
  </section>
</template>

<script>
/* eslint-disable */
import AskQuestionEmployee from '../common/customer-support/AskQuestion.vue'
export default {
  name: 'AskQuestion',
  components: { AskQuestionEmployee },
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
