<template>
  <div class="mt-5">
    <div class="row mb-3">
      <div class="col-md-3 my-2">
        <!-- <select v-model="getResourceData.sortBy" class="form-control" v-on:change="getResourcesList">
                    <option value="">Sort By</option>
                    <option value="title">Title</option>
                </select> -->
      </div>
      <div class="col-md-3 my-2">
        <!-- <input type="text" v-model="getResourceData.keyword" class="form-control" placeholder="Search"
                    v-on:keyup="getResourcesList" /> -->
      </div>
      <!--<div class="col-md-3 my-2">
          <input type="text" v-model="getResourceData.keyword" class="form-control" placeholder="Search"
                     v-on:keyup="getResourcesList" />
       </div> -->
      <div class="col-lg-6 col-md-12 my-2 d-flex">
        <button v-if="user.role == 'ADMIN'" class="ml-auto btn btn-primary float-right" v-b-modal.add-opportunity-modal>
          Add New Opportunity
        </button>
      </div>
    </div>
    <div class=" mt-3">
      <div class="table-responsive">
        <table class="table">
          <tr>
            <th v-if="user.role == 'ADMIN'">Company Name</th>
            <th>Content</th>
            <th v-if="user.role == 'ADMIN'">Action</th>
          </tr>
          <tr v-if="opportunitiesLength" v-for="r in opportunities.data" v-bind:key="r.id">
            <td v-if="user.role == 'ADMIN'">
              <span>{{ r.company.map(({company_name}) => company_name).join(',') }}
            </span>
            </td>
            <td>
              <p v-html="r.content"></p>
            </td>
            <td v-if="user.role == 'ADMIN'">

              <div class="d-flex align-items-center p-0" style="min-width: 100px;">
                <a type="button" class="mx-3 d-block" width="24" @click="getOpportunity(r.id)">
                  <img src="../../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24"/>
                </a>
                <a type="button" class="mx-3 d-block" width="24" @click="deleteOpportunity(r.id)">
                  <img src="../../../assets/images/table-delete.svg" alt="table-delete" width="24" height="24"/></a>
              </div>
              <!-- <button class="btn btn-primary" @click="getOpportunity(r.id)"><i class="fa fa-pencil"></i></button>
              <button class="btn btn-danger" @click="deleteOpportunity(r.id)"><i class="fa fa-trash"></i></button> -->
            </td>
          </tr>
          <tr v-if="!opportunitiesLength">
            <td colspan="5">No Data Found</td>
          </tr>
        </table>
      </div>
      <pagination :data="opportunities" @pagination-change-page="getOpportunityList"/>
    </div>
    <!--Add resource modal popup-->
    <Add :getOpportunityList="getOpportunityList"></Add>
    <!--Update resource modal popup-->
    <Edit :getOpportunityList="getOpportunityList" :opportunityUpdate="opportunityUpdate"></Edit>
  </div>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import Add from '../opportunities/Add.vue'
import Edit from '../opportunities/Edit.vue'

export default {
  name: 'List',
  mixins: [AppMixin],
  components: {Add, Edit},
  data () {
    return {
      opportunityUpdate: {
        id: '',
        company: '',
        description: '',
        company: '',
        disabled: false
      },
      opportunities: {},
      opportunitiesLength: ''
    }
  },
  methods: {
    getOpportunity: function (id) {
      let that = this
      Api.getOpportunity(id).then(response => {
        that.opportunityUpdate.id = response.data.res.id
        that.opportunityUpdate.company = response.data.res.company
        that.opportunityUpdate.description = response.data.res.description
        that.$bvModal.show('update-opportunity-modal')
      }).catch((error) => {
        this.$swal({
          icon: 'error',
          title: 'error',
          text: error.response.data.message,
          showConfirmButton: true
        })
      })
    },
    deleteOpportunity: function (id) {
      let that = this
      this.$swal({
        title: 'Are you sure?',
        text: 'All the related info will be deleted, you wont be able to revert !',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes Delete it!',
        cancelButtonText: 'No, Keep it!',
        showCloseButton: true,
        showLoaderOnConfirm: true
      }).then((result) => {
        if (result.value) {
          Api.deleteOpportunity(id).then(response => {
            this.$swal({
              icon: 'success',
              title: 'Success',
              text: 'Opportunity deleted successfully',
              showConfirmButton: true
            }).then(function () {
              that.getOpportunityList()
            })
          }).catch((error) => {
            this.$swal({
              icon: 'error',
              title: 'error',
              text: error.response.data.message,
              showConfirmButton: true
            }).then(function () {
              that.opportunity.disabled = false
            })
          })
        }
      })
    },
    getOpportunityList: function (page = 1) {
      let that = this
      Api.getOpportunityList(page).then(response => {
        that.opportunities = response.data.res
        that.opportunitiesLength = that.opportunities.data.length
      }).catch((error) => {
        this.$swal({
          icon: 'error',
          title: 'error',
          text: error.response.data.message,
          showConfirmButton: true
        }).then(function () {
          that.opportunity.disabled = false
        })
      })
    },
  },
  mounted () {
    this.getOpportunityList()
  }
}
</script>
