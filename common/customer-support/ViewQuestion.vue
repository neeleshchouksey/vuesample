<template>
  <section class="employee-todo-view-section half-cut-bg">
    <div class="container">
        <h4 >Question:</h4>
        <p>{{ Question.description }}</p>
        <h4>Response: </h4>
        <p v-html="Question.response"></p>
        <h4 >Company Name:</h4>
        <p>{{ Question.company }}</p>
    </div>
  </section>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'

export default {
  name: 'ViewQuestion',
  mixins: [AppMixin],
  data() {
    return {
      Question:{
        'id':'',
        'description':'',
        'response':'',
        'company':''
      }
    }
  },
  methods: {
    getQuestion: function (id) {
      let that = this;
      Api.getQuestion(id).then(response => {
        that.Question.id = response.data.res.id
        that.Question.description = response.data.res.description
        that.Question.response = response.data.res.response
        that.Question.company = response.data.res.company_name
      }).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
  },
  mounted() {
    this.getQuestion(this.$route.params.id)
  }
}
</script>