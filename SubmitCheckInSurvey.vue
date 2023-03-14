<template>
  <section class="login-section">
    <div class="login-inner">
      <div class="">
        <div class="login-bg-text">
          <div class="left-content">
            <h1>Mpact International</h1>
            <h5>Check-In Survey</h5>
            <CustomSurvey v-if="pg.length" :surveyProp="pg" :submitPopupSurvey="submitCheckInSurvey"></CustomSurvey>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Api from '../router/api'
import CustomSurvey from '../components/employee/CustomSurvey.vue'

/* eslint-disable */
export default {
  name: 'SubmitCheckInSurvey',
  data() {
    return {
     
    }
  },
  components: { CustomSurvey },
  data() {
    return {
      pg: [],
    }
  },
  methods: {
    getCheckInSurveyQuestions: function () {
      Api.getCheckInSurveyQuestions(this.$route.params.link).then(response => {
        this.pg = response.data.res;
      });
    },
    submitCheckInSurvey: function (survey) {
      let data = JSON.stringify(survey.data, null, 3)
      this.surveyRes = JSON.parse(data)
      Api.submitCheckInSurvey(this.surveyRes,this.$route.params.link).then(response => {

      });
    }
  },
  created() {
    this.getCheckInSurveyQuestions();
  },
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
