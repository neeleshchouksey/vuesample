<template>
  <section class="employee-company-feedback-section">
      <h1 class="page-title text-left">Feeback to <span>company admin</span></h1>
       <div class="card-box">
          <textarea class="form-control" id="ask-question" v-model="feedback.description"
            placeholder="Feedback to company admin"></textarea>

        <div class="form-group mt-2">
          <input type="checkbox" v-model="feedback.anonymous" /> Anonymous
        </div>
        <div class="form-group mt-4">
          <button type="button" :disabled="feedback.disabled" class="btn btn-primary"
            @click="submitCompanyFeedback">Submit</button>
        </div>
        </div>
  </section>
</template>

<script>
/* eslint-disable */
import Api from '../../router/api'
export default {
  name: 'CompanyFeedback',
  data() {
    return {
      feedback: {
        description: '',
        anonymous: '',
        disabled: false
      }
    }
  },
  methods: {
    submitCompanyFeedback: function () {
      let that = this
      if (!that.feedback.description) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        })
      } else {
        that.feedback.disabled = true
        Api.submitCompanyFeedback(that.feedback).then(response => {
          that.feedback.disabled = false
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Submitted successfully",
            showConfirmButton: true
          }).then(function () {
            that.feedback.disabled = false
            that.feedback.description = ''
            that.feedback.anonymous = ''
          });
        })
      }
    }
  },
  mounted() {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
