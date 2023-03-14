<template>
  <section class="popup-survey-view-section half-cut-bg">
    <h1 class="page-title text-left">Learning Plan</h1>
    <div class="row">
      <div class="col-md-5">
        <div class="popup-survey-lable">
          <p class="w-100 mb-0 page-sub-title">Plan Title :</p>
          <p class="w-100 pink-color"><strong>{{ planSingle.title }}</strong></p>
        </div>
        <div class="popup-survey-lable">
          <p class="w-100 mb-0 page-sub-title">Plan Description :</p>
          <p class="w-100 pink-color"><strong>{{ planSingle.description }} </strong></p>
        </div>
      </div>
      <div class="col-md-6 ml-auto">
        <div class="popup-survey-lable">
          <p class="w-100 mb-0 page-sub-title">Plan Profile Type :</p>
          <p class="w-100 pink-color"><strong><span>{{ planSingle.profile_type.map(({ name }) => name).join(',') }}
              </span></strong></p>
        </div>
        <div class="popup-survey-lable ">
          <p class="w-100 mb-0 page-sub-title">Plan Image :</p>
          <p class="w-100"><strong>
              <img :src="filePath + '/' + planSingle.image" class="table-img" height="75" width="75" /></strong></p>
        </div>
      </div>
      <div class="row">
        <div class="col-md-3 my-2 d-flex align-items-center">
          <input type="text" v-model="searchData.keyword" class="form-control mb-0 search" placeholder="Search"
            v-on:keyup="getLearningPlanFiles" /><span class="search-icon"></span><a href="javascript:void(0)"
            v-on:click="searchData.keyword = ''; getLearningPlanFiles()" class="link px-2 ">clear</a>
        </div>
        <div class="col-md-3 my-2">
        </div>
      </div>
    </div>
    <div class="table-responsive">
      <table class="table">
        <tr>
          <th>Title<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'title'; searchData.sortOrder = 'asc'; getLearningPlanFiles()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'title'; searchData.sortOrder = 'desc'; getLearningPlanFiles()"></i>
          </th>
          <th>Description<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'title'; searchData.sortOrder = 'asc'; getLearningPlanFiles()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'title'; searchData.sortOrder = 'desc'; getLearningPlanFiles()"></i>
          </th>
          <th>File</th>
        </tr>
        <tr v-if="planFiles.length" v-for="lp in planFiles" v-bind:key="lp.id">
          <td>{{ lp.title }}</td>
          <td>{{ lp.description }}</td>
          <td><a class="cursor-pointer links" @click="downloadLearningPlanFile(lp.id, lp.image)">Download</a></td>
        </tr>
        <tr v-if="!planFiles.length">
          <td colspan="5">No Data Found</td>
        </tr>
      </table>
    </div>

  </section>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'

export default {
  name: 'LearningPlan',
  mixins: [AppMixin],
  data() {
    return {
      filePath: '',
      hideFooter: true,
      totalDescriptionChar: 300,
      totalTitleChar: 30,
      remainingDescriptionChar: 300,
      remainingTitleChar: 30,

      searchData: {
        'sortBy': '',
        'sortOrder': '',
        'keyword': ''
      }
    }
  },
  components: {},
  methods: {

    deleteLearningPlanFile: function (id) {
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
          Api.deleteLearningPlanFile(id).then(response => {
            this.$swal({
              icon: 'success',
              title: 'Success',
              text: 'Todo deleted successfully',
              showConfirmButton: true
            }).then(function () {
              that.getLearningPlanFiles()
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
    getLearningPlanFile: function (data) {
      this.planUpdate.id = data.id
      this.planUpdate.title = data.title
      this.planUpdate.description = data.description
      this.$bvModal.show('update-modal')
    },
    imageOnChange: function (e) {
      let that = this
      this.plan.image = this.$refs.image.files[0]
    },
    imageOnChangeUpdate: function (e) {
      let that = this
      this.planUpdate.image = this.$refs.imageUpdate.files[0]
    },
    addLearningPlanFile: function (e) {
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
        formData.append('image', that.plan.image)
        formData.append('description', that.plan.description)
        formData.append('title', that.plan.title)
        formData.append('my_learning_plan_id', that.$route.params.id)
        Api.addLearningPlanFile(formData).then(response => {
          that.plan.disabled = false

          let headers = {
            'Content-Type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
          }
          this.$swal({
            icon: 'success',
            title: 'Success',
            text: 'Learning Plan File created successfully',
            showConfirmButton: true
          }).then(function () {
            that.$bvModal.hide('add-modal')
            that.plan.title = ''
            that.plan.description = ''
            that.$refs.image.value = null
            that.getLearningPlanFiles()
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
    updateLearningPlanFile: function (e) {
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
        formData.append('image', that.planUpdate.image)
        formData.append('description', that.planUpdate.description)
        formData.append('title', that.planUpdate.title)
        formData.append('id', that.planUpdate.id)
        Api.updateLearningPlanFile(formData).then(response => {
          that.planUpdate.disabled = false

          let headers = {
            'Content-Type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
          }
          this.$swal({
            icon: 'success',
            title: 'Success',
            text: 'Learning Plan File updated successfully',
            showConfirmButton: true
          }).then(function () {
            that.$bvModal.hide('update-modal')
            that.planUpdate.title = ''
            that.planUpdate.description = ''
            that.$refs.imageUpdate.value = null
            that.getLearningPlanFiles()
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
    getLearningPlanFiles: function () {
      let that = this
      Api.getLearningPlanFiles(that.$route.params.id, that.searchData).then(response => {
        that.planSingle.profile_type = response.data.res.profile_type
        that.planSingle.title = response.data.res.title
        that.planSingle.description = response.data.res.description
        that.planSingle.image = response.data.res.image
        that.planFiles = response.data.res.files
        that.filePath = response.data.path
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
    descriptionCharCount: function () {
      this.remainingDescriptionChar = this.totalDescriptionChar - this.plan.description.length
    },
    titleCharCount: function () {
      this.remainingTitleChar = this.totalTitleChar - this.plan.title.length
    },
    descriptionCharCountUpdate: function () {
      this.remainingDescriptionChar = this.totalDescriptionChar - this.planUpdate.description.length
    },
    titleCharCountUpdate: function () {
      this.remainingTitleChar = this.totalTitleChar - this.planUpdate.title.length
    },
  },
  mounted() {
    this.getProfileTypeList()
    this.getLearningPlanFiles()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
