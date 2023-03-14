<template>

  <section class="admin-steps-configuration-section half-cut-bg">
    <h1 class="page-title text-left mt-0"><span> Steps</span></h1>
    <div class="mt-5">
      <div class="row mb-3 align-items-center">
        <div class="col-md-4 my-2 d-flex align-items-center ">
          <input type="text" v-model="getStepData.keyword" class="form-control m-0 search" placeholder="Search"
            v-on:keyup="getStepsList" /><span class="search-icon"></span><a href="javascript:void(0)"
            v-on:click="getStepData.keyword = ''; getStepsList()" class="link px-2 mb-3">clear</a>
        </div>
        <div class="col-md-3 my-2">
          <!-- <select v-model="getStepData.sortBy" class="form-control" v-on:change="getStepsList">
            <option value="">Sort By</option>
            <option value="title">Title</option>
          </select> -->
        </div>
        <div class="col-md-5 d-flex my-2">
          <button class="btn btn-primary float-right ml-auto" v-b-modal.add-step-modal>Add New Step</button>
        </div>
      </div>
      <div class="table-responsive">
        <table class="table">
          <tr>
            <th>Title<i class="fa-solid fa-arrow-up"
              @click="getStepData.sortBy = 'title'; getStepData.sortOrder='asc';getStepsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getStepData.sortBy = 'title'; getStepData.sortOrder='desc';getStepsList()"></i></th>
            <th>Overview<i class="fa-solid fa-arrow-up"
              @click="getStepData.sortBy = 'overview'; getStepData.sortOrder='asc';getStepsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getStepData.sortBy = 'overview'; getStepData.sortOrder='desc';getStepsList()"></i></th>
            <th>Image</th>
            <th>Action</th>
          </tr>
          <tr v-if="stepsLength" v-for="r in stepsList" v-bind:key="r.id">
            <td>{{ r.title }}</td>
            <td>{{ r.overview }}</td>
            <td><img :src="filePath + '/' + r.image" class="table-img" height="70" width="70" /></td>
            <td>

              <div class="d-flex align-items-center p-0" style="min-width: 150px;">
                <a type="button" class="mx-3 d-block" width="24" @click="getStep(r.id)">
                  <img src="../../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
                </a>
                <a type="button" class="mx-3 d-block" width="24" @click="deleteStep(r.id)">
                  <img src="../../../assets/images/table-delete.svg" alt="table-delete" width="24" height="24" /></a>
                <router-link class="mx-3 d-block" :to="'/admin/view-step/' + r.id">
                  <img src="../../../assets/images/table-eye.svg" alt="table-eye" width="24" height="24" />
                </router-link>
              </div>

              <!--<button class="btn btn-primary" @click="getStep(r.id)"><i class="fa fa-pencil"></i></button>
            <button class="btn btn-danger" @click="deleteStep(r.id)"><i class="fa fa-trash"></i></button>
            <router-link class="btn btn-primary" :to="'/admin/view-step/' + r.id"><i class="fa fa-eye"></i>
            </router-link>-->
            </td>
          </tr>
          <tr v-if="!stepsLength">
            <td colspan="5">No Data Found</td>
          </tr>
        </table>

      </div>
    </div>
    <!--Add step modal popup-->
    <b-modal id="add-step-modal" title="Add New Step" :hide-footer=hideFooter size="lg" no-fade no-enforce-focus>
      <form enctype="multipart/form-data" @submit="addStep">
        <div id="details">
          <div class="form-group">
            <label>Title <span class="err">*</span></label>
            <input type="text" class="form-control" id="title" placeholder="Title" v-model="step.title">
          </div>
          <div class="form-group">
            <label>Overview<span class="err">*</span></label>
            <textarea class="form-control" id="overview" placeholder="Overview" v-model="step.overview"></textarea>
          </div>
          <div class="form-group">
            <label>Tools and Review<span class="err">*</span></label>
            <vue2-tinymce-editor v-model="step.description" placeholder="Tools and Review"></vue2-tinymce-editor>
          </div>
          <div class="form-group">
            <label>Upload Image <span class="err">*</span></label>
            <input type="file" class="form-control" id="image" ref="image" @change="fileOnChange">
          </div>
          <button type="submit" class="btn btn-primary" :disabled="step.disabled">Submit</button>
        </div>
      </form>
    </b-modal>
    <!--Update step modal popup-->
    <b-modal id="update-step-modal" title="Update Step" :hide-footer=hideFooter size="lg" no-fade no-enforce-focus>
      <form enctype="multipart/form-data" @submit="updateStep">
        <div id="details">
          <div class="form-group">
            <label>Title <span class="err">*</span></label>
            <input type="text" class="form-control" id="title" placeholder="Title" v-model="stepUpdate.title">
          </div>
          <div class="form-group">
            <label>Overview<span class="err">*</span></label>
            <textarea class="form-control" id="overview" placeholder="Overview"
              v-model="stepUpdate.overview"></textarea>
          </div>
          <div class="form-group">
            <label>Tools and Review<span class="err">*</span></label>
            <vue2-tinymce-editor v-model="stepUpdate.description" placeholder="Tools and Review"></vue2-tinymce-editor>
          </div>
          <div class="form-group">
            <label>Upload Image <span class="err">*</span></label>
            <input type="file" class="form-control" id="image" ref="imageUpdate" @change="fileOnChangeUpdate">
          </div>
          <button type="submit" class="btn btn-primary" :disabled="step.disabled">Submit</button>
        </div>
      </form>
    </b-modal>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import { Vue2TinymceEditor } from "vue2-tinymce-editor";

export default {
  name: 'StepConfiguration',
  mixins: [AppMixin],
  components: {
    Vue2TinymceEditor
  },
  data() {
    return {
      hideFooter: true,
      stepsList: {},
      stepsLength: 0,
      step: {
        'title': '',
        'overview': '',
        'description': '',
        'image': '',
        'disabled': false,
      },

      getStepData: {
        'sortBy': '',
        'keyword': '',
        'sortOrder':''
      },
      filePath: ''
    }
  },
  methods: {
    fileOnChange: function (e) {
      this.step.image = this.$refs.image.files[0];
    },
    fileOnChangeUpdate: function () {
      this.stepUpdate.image = this.$refs.imageUpdate.files[0];
    },
    getStepsList: function (page = 1) {
      let that = this
      Api.getStepsList(page, that.getStepData).then(response => {
        let that = this
        that.stepsList = response.data.res
        that.stepsLength = that.stepsList.length
        that.filePath = response.data.path
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
    addStep: function (e) {
      e.preventDefault();
      let that = this;
      if (!that.step.title || !that.step.description || !that.step.image) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields fields",
          showConfirmButton: true
        });
      } else {
        let that = this
        that.step.disabled = true;
        const formData = new FormData();
        formData.append('image', that.step.image);
        formData.append('title', that.step.title)
        formData.append('overview', that.step.overview)
        formData.append('description', that.step.description)
        let headers = {
          'Content-Type': 'multipart/form-data',
          'Access-Control-Allow-Origin': '*'
        }
        Api.addStep(formData, headers).then(response => {
          that.step.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Step created successfully",
            showConfirmButton: true
          }).then(function () {
            that.step.title = ''
            that.step.description = ''
            that.$refs.image.value = null;
            that.$bvModal.hide('add-step-modal')
            that.getStepsList()
          });
        }
        ).catch((error) => {
          that.step.disabled = false;
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.step.disabled = false;
          });
        });
      }
    },
    deleteStep: function (id) {
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
          Api.deleteStep(id).then(response => {
            this.$swal({
              icon: "success",
              title: "success",
              text: "Deleted Successfully",
              showConfirmButton: true
            }).then(() => {
              that.getStepsList()
            });
          }
          ).catch((error) => {
            this.$swal({
              icon: "error",
              title: "error",
              text: error.response.data.message,
              showConfirmButton: true
            });
          });
        }
      })
    },
    updateStep: function (e) {
      e.preventDefault()
      let that = this;
      if (!that.stepUpdate.title || !that.stepUpdate.description || !that.stepUpdate.image) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else {
        that.stepUpdate.disabled = true;
        const formData = new FormData();
        formData.append('id', that.stepUpdate.id);
        formData.append('image', that.stepUpdate.image)
        formData.append('title', that.stepUpdate.title)
        formData.append('overview', that.stepUpdate.overview)
        formData.append('description', that.stepUpdate.description)
        let headers = {
          'Content-Type': 'multipart/form-data',
          'Access-Control-Allow-Origin': '*'
        }
        Api.updateStep(formData, headers).then(response => {
          that.stepUpdate.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Step details updated successfully",
            showConfirmButton: true
          }).then(function () {
            that.stepUpdate.disabled = false;
            that.$bvModal.hide('update-step-modal')
            that.getStepsList()
          });
        }).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.step.disabled = false;
          });
        });
      }
    },
  },
  mounted() {
    this.getStepsList()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
