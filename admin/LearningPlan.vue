<template>
  <section class="admin-learning-plan-section half-cut-bg">
    <h1 class="page-title text-left mt-0">Learning <span>Plan</span></h1>
    <div class="row">
      <div class="col-md-3 my-2 d-flex align-items-center">
        <input type="text" v-model="searchData.keyword" class="form-control mb-0 search" placeholder="Search"
          v-on:keyup="getLearningPlanList" /><span class="search-icon"></span><a href="javascript:void(0)"
          v-on:click="searchData.keyword = ''; getLearningPlanList()" class="link px-2 ">clear</a>
      </div>
      <div class="col-md-3 my-2">
      </div>
      <div class="col-md-6 d-flex my-2" v-if="user.role == 'ADMIN'">
        <button class="btn btn-primary float-right ml-auto" v-b-modal.add-modal>Add New Learning Plan</button>
      </div>
    </div>
    <div class="table-responsive">
      <table class="table">
        <tr>
          <th>Image</th>
          <th>Title<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'title'; searchData.sortOrder = 'asc'; getLearningPlanList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'title'; searchData.sortOrder = 'desc'; getLearningPlanList()"></i>
          </th>
          <th>Description<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'description'; searchData.sortOrder = 'asc'; getLearningPlanList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'description'; searchData.sortOrder = 'desc'; getLearningPlanList()"></i>
          </th>
          <th>Profile Type</th>
          <th>Action</th>
        </tr>
        <tr v-if="learningPlanLength" v-for="lp in learningPlan.data" v-bind:key="lp.id">
          <td><img :src="learningPlanPath + '/' + lp.image" class="table-img" height="75" width="75" /></td>
          <td>{{ lp.title }}</td>
          <td>{{ lp.description }}</td>
          <td> <span>{{ lp.profile_type.map(({ profile_type }) => profile_type).join(',') }}
            </span></td>
          <td>

            <div class="d-flex align-items-center p-0" style="min-width: 100px;">
              <a type="button" class="mx-3 d-block" width="24" @click="getLearningPlan(lp.id)">
                <img src="../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
              </a>
              <a type="button" class="mx-3 d-block" width="24" @click="deleteLearningPlan(lp.id)">
                <img src="../../assets/images/table-delete.svg" alt="table-delete" width="24" height="24" /></a>
              <router-link class="mx-3 d-block" :to="'/admin/learning-plan/' + lp.id">
                <img src="../../assets/images/table-eye.svg" alt="table-delete" width="24" height="24" />
              </router-link>
            </div>
            <!-- <button class="btn btn-primary" @click="getLearningPlan(lp)"><i class="fa fa-pencil"></i></button>
        <button class="btn btn-danger" @click="deleteLearningPlan(lp.id)"><i
                class="fa fa-trash"></i></button>
        <router-link class="btn btn-primary"
            :to="'/admin/learning-plan/' + lp.id"><i class="fa fa-eye"></i></router-link>-->
          </td>
        </tr>
        <tr v-if="!learningPlanLength">
          <td colspan="5">No Data Found</td>
        </tr>
      </table>
    </div>
    <b-modal id="add-modal" title="Add New Learning Plan" :hide-footer=hideFooter>
      <form enctype="multipart/form-data">
        <div id="details">
          <div class="form-group" v-if="user.role == 'ADMIN'">
            <label>Select Profile Type <span class="err">*</span></label>
            <multiselect v-model="plan.profile_type" :options="profileTypeListMultiselect" group-values="values"
              group-label="selectAll" :multiple="true" :group-select="true" placeholder="Type to search" track-by="name"
              label="name">
              <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
            </multiselect>
          </div>
          <div class="form-group" v-if="user.role == 'ADMIN'">
            <label>Select Resources <span class="err">*</span></label>
            <multiselect v-model="plan.resources" :options="resourcesListMultiselect" group-values="values"
              group-label="selectAll" :multiple="true" :group-select="true" placeholder="Type to search" track-by="name"
              label="name">
              <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
            </multiselect>
          </div>
          <div class="form-group">
            <label>Title <span class="err">*</span></label>
            <input type="text" class="form-control" id="title" placeholder="Title" v-model="plan.title">
          </div>
          <div class="form-group">
            <label>Description<span class="err">*</span></label>
            <textarea class="form-control" id="description" placeholder="Description"
              v-model="plan.description"></textarea>
          </div>
          <div class="form-group">
            <label>Image<span class="err">*</span></label>
            <input type="file" class="form-control" id="image" ref="image" @change="imageOnChange">
          </div>
          <button type="button" class="btn btn-primary" @click="addLearningPlan" :disabled="plan.disabled">Submit
          </button>
        </div>
      </form>
    </b-modal>
    <b-modal id="update-modal" title="Update Learning Plan" :hide-footer=hideFooter>
      <form enctype="multipart/form-data">
        <div id="details">
          <div class="form-group" v-if="user.role == 'ADMIN'">
            <label>Select Profile Type <span class="err">*</span></label>
            <!-- <select class="form-control" v-model="planUpdate.profile_type">
              <option value="">Select</option>
              <option v-for="pt in profileType" v-bind:key="pt.id" :value="pt.id">
                {{ pt.profile_type }}
              </option>
            </select> -->

            <multiselect v-model="planUpdate.profile_type" :options="profileTypeListMultiselectUpdate"
              group-values="values" group-label="selectAll" :multiple="true" :group-select="true"
              placeholder="Type to search" track-by="name" label="name">
              <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
            </multiselect>
          </div>
          <div class="form-group" v-if="user.role == 'ADMIN'">
            <label>Select Resources <span class="err">*</span></label>
            <multiselect v-model="planUpdate.resources" :options="resourcesListMultiselect" group-values="values"
              group-label="selectAll" :multiple="true" :group-select="true" placeholder="Type to search" track-by="name"
              label="name">
              <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
            </multiselect>
          </div>
          <div class="form-group">
            <label>Title <span class="err">*</span></label>
            <input type="text" class="form-control" id="title" placeholder="Title" v-model="planUpdate.title">
          </div>
          <div class="form-group">
            <label>Description<span class="err">*</span></label>
            <textarea class="form-control" id="description" placeholder="Description"
              v-model="planUpdate.description"></textarea>
          </div>
          <div class="form-group">
            <label>Image<span class="err">*</span></label>
            <input type="file" class="form-control" id="image" ref="imageUpdate" @change="imageOnChangeUpdate">
          </div>
          <button type="button" class="btn btn-primary" @click="updateLearningPlan"
            :disabled="planUpdate.disabled">Submit
          </button>
        </div>
      </form>
    </b-modal>
  </section>
</template>
<style src="vue-multiselect/dist/vue-multiselect.min.css">

</style>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'

export default {
  name: 'LearningPlan',
  mixins: [AppMixin],
  data() {
    return {
      hideFooter: true,
      plan: {
        'profile_type': '',
        'title': '',
        'description': '',
        'image': '',
        'disabled': false,
        'resources':''
      },
      planUpdate: {
        'id': '',
        'profile_type': '',
        'title': '',
        'description': '',
        'image': '',
        'disabled': false,
        'resources':''
      },
      profileTypeListMultiselectUpdate: [
        {
          selectAll: 'Select All',
          values: []
        }
      ],
      resourcesListMultiselect: [
        {
          selectAll: 'Select All',
          values: []
        }
      ],
      searchData: {
        'sortBy': '',
        'sortOrder': '',
        'keyword': ''
      }
    }
  },
  components: {},
  methods: {
    getLearningPlan: function (id) {
      Api.getLearningPlan(id).then(response => {
        this.planUpdate.id = response.data.res.id
        this.planUpdate.title = response.data.res.title
        this.planUpdate.description = response.data.res.description
        this.planUpdate.profile_type = response.data.res.profile_type
        this.planUpdate.resources = response.data.res.resources
        this.getProfileTypeListMultiselectUpdate()
        this.$bvModal.show('update-modal')
      })

    },
    deleteLearningPlan: function (id) {
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
          Api.deleteLearningPlan(id).then(response => {
            this.$swal({
              icon: 'success',
              title: 'Success',
              text: 'Learning Plan deleted successfully',
              showConfirmButton: true
            }).then(function () {
              that.getLearningPlanList()
              this.getProfileTypeListMultiselect()
              this.getProfileTypeListMultiselectUpdate()

            })
          }).catch((error) => {
            this.$swal({
              icon: 'error',
              title: 'error',
              text: error.response.data.message,
              showConfirmButton: true
            }).then(function () {
              that.todo.disabled = false
            })
          })
        }
      })
    },
    imageOnChange: function (e) {
      let that = this
      this.plan.image = this.$refs.image.files[0]
    },
    imageOnChangeUpdate: function (e) {
      let that = this
      this.planUpdate.image = this.$refs.imageUpdate.files[0]
    },
    addLearningPlan: function (e) {
      e.preventDefault()
      let that = this
      if (!that.plan.title || !that.plan.description || !that.plan.image) {
        this.$swal({
          icon: 'error',
          title: 'error',
          text: 'Please all required fields',
          showConfirmButton: true
        })
      } else {
        that.plan.disabled = true
        const formData = new FormData()
        formData.append('profile_type', JSON.stringify(that.plan.profile_type))
        formData.append('resources', JSON.stringify(that.plan.resources))
        formData.append('image', that.plan.image)
        formData.append('description', that.plan.description)
        formData.append('title', that.plan.title)
        Api.addLearningPlan(formData).then(response => {
          that.plan.disabled = false

          let headers = {
            'Content-Type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
          }
          this.$swal({
            icon: 'success',
            title: 'Success',
            text: 'Learning Plan created successfully',
            showConfirmButton: true
          }).then(function () {
            that.$bvModal.hide('add-modal')
            that.plan.profile_type = ''
            that.plan.title = ''
            that.plan.description = ''
            that.$refs.image.value = null
            that.getLearningPlanList()
            this.profileTypeListMultiselectUpdate()
            this.profileTypeListMultiselect()
          })
        }
        ).catch((error) => {
          that.plan.disabled = false
          this.$swal({
            icon: 'error',
            title: 'error',
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.plan.disabled = false
          })
        })
      }
    },
    updateLearningPlan: function (e) {
      e.preventDefault()
      let that = this
      if (!that.planUpdate.title || !that.planUpdate.description) {
        this.$swal({
          icon: 'error',
          title: 'error',
          text: 'Please all required fields',
          showConfirmButton: true
        })
      } else {
        that.plan.disabled = true
        const formData = new FormData()
        formData.append('profile_type', JSON.stringify(that.planUpdate.profile_type))
        formData.append('resources', JSON.stringify(that.planUpdate.resources))
        formData.append('image', that.planUpdate.image)
        formData.append('description', that.planUpdate.description)
        formData.append('title', that.planUpdate.title)
        formData.append('id', that.planUpdate.id)
        Api.updateLearningPlan(formData).then(response => {
          that.planUpdate.disabled = false

          let headers = {
            'Content-Type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
          }
          this.$swal({
            icon: 'success',
            title: 'Success',
            text: 'Learning Plan updated successfully',
            showConfirmButton: true
          }).then(function () {
            that.$bvModal.hide('update-modal')
            that.planUpdate.profile_type = ''
            that.planUpdate.title = ''
            that.planUpdate.description = ''
            that.$refs.imageUpdate.value = null
            that.getLearningPlanList()
            this.profileTypeListMultiselectUpdate()
            this.profileTypeListMultiselect()
          })
        }
        ).catch((error) => {
          that.plan.disabled = false
          this.$swal({
            icon: 'error',
            title: 'error',
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.plan.disabled = false
          })
        })
      }
    },
    getResourcesListMultiselect: function () {
      let that = this
      Api.getResourcesListMultiselect().then(response => {
        let that = this
        that.resourcesListMultiselect[0].values = response.data.res
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
    getProfileTypeListMultiselectUpdate: function () {
      let that = this
      Api.getProfileTypeListMultiselectUpdate().then(response => {
        let that = this
        that.profileTypeListMultiselectUpdate[0].values = response.data.res
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
    getLearningPlanList: function (page = 1) {
      let that = this
      Api.getLearningPlanList(page, that.searchData).then(response => {
        that.learningPlan = response.data.res
        that.learningPlanLength = that.learningPlan.data.length
        that.learningPlanPath = response.data.path
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
  },
  mounted() {
    this.getProfileTypeListMultiselect()
    this.getProfileTypeListMultiselectUpdate()
    this.getLearningPlanList()
    this.getResourcesListMultiselect()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
