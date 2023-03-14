<template>
  <section class="admin-workshops-section half-cut-bg">
    <!-- <h1 class="page-title text-left mt-0">ASK YOUR <span>CARE TEAM</span></h1> -->
    <div class="table-responsive">
      <table class="table">
        <tr>
          <th>Name <i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'first_name'; searchData.sortOrder = 'asc'; getQuestionList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'first_name'; searchData.sortOrder = 'desc'; getQuestionList()"></i></th>
          <!-- <th>Company Name <i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'company_name'; searchData.sortOrder='asc';getQuestionList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'company_name'; searchData.sortOrder='desc';getQuestionList()"></i></th> -->
          <th>Description <i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'description'; searchData.sortOrder = 'asc'; getQuestionList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'description'; searchData.sortOrder = 'desc'; getQuestionList()"></i></th>
          <th>Date <i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'created_at'; searchData.sortOrder = 'asc'; getQuestionList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'created_at'; searchData.sortOrder = 'desc'; getQuestionList()"></i></th>
          <th>Response <i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'response'; searchData.sortOrder = 'asc'; getQuestionList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'response'; searchData.sortOrder = 'desc'; getQuestionList()"></i></th>
          <th>Action</th>
        </tr>
        <tr v-if="questionListLength" v-for="r in questionList.data" v-bind:key="r.id">
          <td>{{ r.first_name }} {{ r.last_name }}</td>
          <!-- <td>{{ r.company_name }}</td> -->
          <td>
            <p>{{ r.description | truncate(20)}}</p>
          </td>
          <td>{{ r.created_at | timeAgo }}</td>
          <td><p v-html="r.response ? (r.response) : 'NA'"></p></td>
          <td>
            <button type="button" class="btn btn-primary"
              v-if="!r.forward_to_admin && (user.role == 'ADMIN' || (company && company.role == 'COMPANY_ADMIN'))"
              @click="forwardToAdmin(r.id)">Forward to
              Admin</button>
            <button type="button" class="btn btn-primary"
              v-if="r.forward_to_admin && (user.role == 'ADMIN' || (company && company.role == 'COMPANY_ADMIN'))">Forwarded</button>
            <button type="button" class="btn btn-primary"
              v-if="((user.role == 'ADMIN'  && !r.response)|| (company && company.role == 'COMPANY_ADMIN'  && !r.response && r.company_id != company.id))"
              v-b-modal.response-modal @click="respondQuestion(r.id)">Respond</button>
            <button type="button" class="btn btn-primary"
              v-if="((user.role == 'ADMIN' && r.response)|| (company && company.role == 'COMPANY_ADMIN' && r.response))">Responded</button>
            <button type="button" class="btn btn-primary" @click="archiveQuestion(r.id)">Archive</button>
            <router-link :to="'view-question/' + r.id" class="btn btn-primary">View</router-link>
          </td>
        </tr>
        <tr v-if="!questionListLength">
          <td colspan="5">No Data Found</td>
        </tr>
      </table>
      <pagination :data="questionList" @pagination-change-page="getQuestionList" />
    </div>
    <!-- </div> -->
    <b-modal id="response-modal" size="lg" title="Respond to Question" :hide-footer=hideFooter no-enforce-focus>
      <div class="form-group">
        <label>Response <span class="err">*</span></label>
        <vue2-tinymce-editor v-model="response.response" placeholder="Response"></vue2-tinymce-editor>

        <!-- <textarea class="form-control" id="respond" placeholder="Response" v-model="response.response"></textarea> -->
      </div>
      <button type="button" class="btn btn-primary" @click="submitResponse" :disabled="response.disabled">Submit
      </button>
    </b-modal>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import { Vue2TinymceEditor } from "vue2-tinymce-editor";

import Api from '../../../router/api'
export default {
  name: 'AskYourCareTeam',
  data() {
    return {
      hideFooter: true,
      msg: 'Ask Your Care Team',
      questionList: {},
      questionListLength: 0,
      searchData: {
        'sortBy': '',
        'sortOrder': ''
      },
      response: {
        questionId: '',
        response: '',
        disabled: false
      },
      type:""
    }
  },
  components: {
    Vue2TinymceEditor
  },
  mixins: [AppMixin],
  methods: {
    respondQuestion: function (id) {
      let that = this;
      that.response.questionId = id
    },
    submitResponse: function () {
      let that = this;
      if (!that.response.response) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please enter response",
          showConfirmButton: true
        });
      } else {
        Api.submitResponse(that.response).then(response => {
          that.$swal({
            icon: "success",
            title: "Success",
            text: "Responded successfully",
            showConfirmButton: true
          }).then(function () {
            that.$bvModal.hide('response-modal')
            that.response.response = ''
            that.getQuestionList();
          });
        }).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
          });
        });
      }
    },
    getQuestionList: function (page = 1) {
      let that = this;
      Api.getQuestionList(page, that.searchData).then(response => {
        that.questionList = response.data.res
        that.questionListLength = that.questionList.data.length
      }).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        }).then(function () {
        });
      });
    },
    forwardToAdmin: function (id) {
      let that = this;
      this.$swal({
        title: 'Are you sure?',
        text: 'You want to forward this question to super admin',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes!',
        cancelButtonText: 'No!',
        showCloseButton: true,
        showLoaderOnConfirm: true
      }).then((result) => {
        if (result.value) {
          Api.forwardToAdmin(id).then(response => {
            that.$swal({
              icon: "success",
              title: "Success",
              text: "Forwarded successfully",
              showConfirmButton: true
            }).then(function () {
              that.getQuestionList();
            });
          }).catch((error) => {
            this.$swal({
              icon: "error",
              title: "error",
              text: error.response.data.message,
              showConfirmButton: true
            }).then(function () {
            });
          });
        }
      });
    },
    archiveQuestion: function (id) {
      let that = this;
      this.$swal({
        title: 'Are you sure?',
        text: 'You want to archive this question? You wont be able to revert it',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes!',
        cancelButtonText: 'No!',
        showCloseButton: true,
        showLoaderOnConfirm: true
      }).then((result) => {
        Api.archiveQuestion(id).then(response => {
          that.$swal({
            icon: "success",
            title: "Success",
            text: "Archived successfully",
            showConfirmButton: true
          }).then(function () {
            that.getQuestionList();
          });
        }).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
          });
        });
      });
    },
  },
  mounted() {
    // if(this.user.role == "ADMIN"){
    //   this.type = "admin"
    // }else{
    //   if(this.company.role == "COMPANY_EMP"){
    //     this.type = "employee"
    //   }else{
    //     this.type = "employer"
    //   }
    // }
    this.getQuestionList()
  }
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
