<template>

  <section class="view-step-link half-cut-bg">
    <router-link to="/admin/steps-configuration" class="btn back">
      <!-- <button class="btn-primary"> -->
      <img src="../../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
      <!-- </button> -->
    </router-link>

    <div class="row mt-5 tabs-ui">
      <div class="col-md-12 pricing-section">
        <div class="nav nav-pills me-3" id="v-pills-tab" role="tablist" aria-orientation="vertical">
          <button class="nav-link active" id="v-pills-home-tab" data-bs-toggle="pill" data-bs-target="#v-pills-home"
            type="button" role="tab" aria-controls="v-pills-home" aria-selected="true">Overview</button>
          <button class="nav-link" id="v-pills-profile-tab" data-bs-toggle="pill" data-bs-target="#v-pills-profile"
            type="button" role="tab" aria-controls="v-pills-profile" aria-selected="false">Guide Book</button>
          <button class="nav-link" id="v-pills-messages-tab" data-bs-toggle="pill" data-bs-target="#v-pills-messages"
            type="button" role="tab" aria-controls="v-pills-messages" aria-selected="false">Toolkit</button>
        </div>
      </div>
      <div class="col-md-12">
        <h3 class="view-step-title">Welcome to </h3>
        <h3 class="section-title">{{ stepUpdate.title }}</h3>
        <div class="tab-content" id="v-pills-tabContent">
          <div class="tab-pane fade show active" id="v-pills-home" role="tabpanel" aria-labelledby="v-pills-home-tab">
            <div class="mb-3">
              <p><b class="pink-color">Overview</b></p>
              <p>{{ stepUpdate.overview }}</p>
            </div>
            <div class="desc-box" v-html="stepUpdate.description"></div>
          </div>
          <div class="tab-pane fade" id="v-pills-profile" role="tabpanel" aria-labelledby="v-pills-profile-tab">
            <p><b class="pink-color">Guide Book</b></p>
            <div class="row mt-3">
              <div class="row">
                <form @submit="uploadGuideBook" enctype="multipart/form-data">
                  <div class="form-group">
                    <label><b>Upload Guide Book</b></label>
                    <input type="file" class="form-control" id="guideBook" ref="guideBook"
                      @change="guideBookOnChange()" />
                  </div>
                  <div class="form-group">
                    <button type="submit" class="btn btn-primary" :disabled="step.disabled">Upload</button>
                  </div>
                </form>
              </div>
              <div class="row mt-5">
                <h5>Uploaded Guide Book</h5>
                <embed v-if="stepUpdate.guideBook" :src="toolkitPath + '/' + stepUpdate.guideBook" width="100%"
                  height="800px" />
                <p v-if="!stepUpdate.guideBook">Guide Book Not uploaded </p>
              </div>
            </div>
          </div>
          <div class="tab-pane fade" id="v-pills-messages" role="tabpanel" aria-labelledby="v-pills-messages-tab">
            <p><b class="pink-color">Toolkit</b></p>
            <div class="row">
              <div class="col-md-3 my-2 d-flex align-items-center">
                <input type="text" v-model="searchData.keyword" class="form-control mb-0 search"
                  placeholder="Search" v-on:keyup="getToolkitList" /><span class="search-icon"></span><a href="javascript:void(0)"
                  v-on:click="searchData.keyword = '';getToolkitList()" class="link px-2 ">clear</a>
              </div>
              <div class="col-md-3 my-2">
              </div>
              <div class="col-md-6 d-flex my-2" v-if="user.role == 'ADMIN'">
                <button class="btn btn-primary ml-auto" v-b-modal.add-modal>Upload Toolkit</button>
              </div>
            </div>
            <div class="table-responsive">
              <table class="table">
                <tr>
                  <th>Title<i class="fa-solid fa-arrow-up"
                      @click="searchData.sortBy = 'title'; searchData.sortOrder='asc';getToolkitList()"></i>
                    <i class="fa-solid fa-arrow-down"
                      @click="searchData.sortBy = 'title'; searchData.sortOrder='desc';getToolkitList()"></i>
                  </th>
                  <th>Description<i class="fa-solid fa-arrow-up"
                      @click="searchData.sortBy = 'title'; searchData.sortOrder='asc';getToolkitList()"></i>
                    <i class="fa-solid fa-arrow-down"
                      @click="searchData.sortBy = 'title'; searchData.sortOrder='desc';getToolkitList()"></i>
                  </th>
                  <th>File</th>
                  <th>Action</th>
                </tr>
                <tr v-if="toolkitList.data.length" v-for="tk in toolkitList.data" v-bind:key="tk.id">
                  <td>{{ tk.title }}</td>
                  <td>{{ tk.description }}</td>
                  <td><a class="cursor-pointer links" @click="downloadToolkit(tk.id, tk.file)">Download</a>
                  </td>
                  <td>
                    <div class="d-flex align-items-center p-0" style="min-width: 100px;">

                      <a type="button" class="mx-3 d-block" width="24" @click="getToolkit(tk)">
                        <img src="../../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
                      </a>
                      <a type="button" class="mx-3 d-block" width="24" @click="deleteToolkit(tk.id)">
                        <img src="../../../assets/images/table-delete.svg" alt="table-delete" width="24" height="24" />
                      </a>
                    </div>
                    <!-- <button class="btn btn-primary" @click="getLearningPlanFile(lp)"><i
                    class="fa fa-pencil"></i></button>
            <button class="btn btn-danger" @click="deleteLearningPlanFile(lp.id)"><i
                    class="fa fa-trash"></i></button> -->
                  </td>
                </tr>
                <tr v-if="!toolkitList.data.length">
                  <td colspan="5">No Data Found</td>
                </tr>
              </table>
            </div>
            <b-modal id="add-modal" title="Upload Toolkit" :hide-footer=hideFooter>
              <form @submit="uploadToolkit" enctype="multipart/form-data">
                <div class="form-group">
                  <label><b>Title</b></label>
                  <input type="text" class="form-control" id="title" v-model="toolkit.title" />
                </div>
                <div class="form-group">
                  <label><b>Description</b></label>
                  <textarea type="text" class="form-control" id="description" v-model="toolkit.description"></textarea>
                </div>
                <div class="form-group">
                  <label><b>Upload Toolkit Files</b></label>
                  <input type="file" class="form-control" id="file" ref="file" @change="fileOnChange()" />
                </div>
                <div class="form-group">
                  <button type="submit" class="btn btn-primary" :disabled="toolkit.disabled">Upload</button>
                </div>
              </form>
            </b-modal>

            <b-modal id="update-modal" title="Update Toolkit" :hide-footer=hideFooter>
              <form @submit="updateToolkit" enctype="multipart/form-data">
                <div class="form-group">
                  <label><b>Title</b></label>
                  <input type="text" class="form-control" id="title" v-model="toolkitUpdate.title" />
                </div>
                <div class="form-group">
                  <label><b>Description</b></label>
                  <textarea type="text" class="form-control" id="description" v-model="toolkitUpdate.description"></textarea>
                </div>
                <div class="form-group">
                  <label><b>Upload Toolkit Files</b></label>
                  <input type="file" class="form-control" id="file" ref="fileUpdate" @change="fileOnChangeUpdate()" />
                </div>
                <div class="form-group">
                  <button type="submit" class="btn btn-primary" :disabled="toolkitUpdate.disabled">Upload</button>
                </div>
              </form>
            </b-modal>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'

export default {
  name: 'ViewStep',
  mixins: [AppMixin],
  data() {
    return {
      step: {
        'disabled': false,
        'file': '',
        'guideBook': '',
      },
      toolkitList:[],
      toolkit: {
        'title': '',
        'description': '',
        'file': '',
        'disabled': false,
      },
      toolkitUpdate: {
        'title': '',
        'description': '',
        'file': '',
        'disabled': false,
      },
      searchData: {
        'sortBy': '',
        'sortOrder': '',
        'keyword': ''
      },
      hideFooter: true,
    }
  },
  methods: {
    fileOnChange: function (e) {
      this.toolkit.file = this.$refs.file.files[0];
    },
    fileOnChangeUpdate: function (e) {
      this.toolkitUpdate.file = this.$refs.fileUpdate.files[0];
    },
    guideBookOnChange: function (e) {
      this.step.guideBook = this.$refs.guideBook.files[0];
    },
    uploadToolkit: function (e) {
      e.preventDefault();
      let that = this
      const formData = new FormData();
      formData.append('file', that.toolkit.file);
      formData.append('id', this.$route.params.id);
      formData.append('title', that.toolkit.title);
      formData.append('description', that.toolkit.description);

      that.toolkit.disabled = true;
      let headers = {
        'Content-Type': 'multipart/form-data',
        'Access-Control-Allow-Origin': '*'
      }
      Api.uploadToolkit(formData, headers).then(response => {
        this.$swal({
          icon: "success",
          title: "Success",
          text: "File Uploaded Successfully",
          showConfirmButton: true
        }).then(function () {
          that.toolkit.disabled = false;
          that.toolkit.title = ''
          that.toolkit.description = ''
          that.$refs.file.value = null
          that.$bvModal.hide('add-modal')
          that.getToolkitList()
        });
      }
      ).catch((error) => {
        that.toolkit.disabled = false;
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
    uploadGuideBook: function (e) {
      e.preventDefault();
      let that = this
      const formData = new FormData();
      formData.append('guideBook', that.step.guideBook);
      formData.append('id', this.$route.params.id)
      that.step.disabled = true;
      let headers = {
        'Content-Type': 'multipart/form-data',
        'Access-Control-Allow-Origin': '*'
      }
      Api.uploadGuideBook(formData, headers).then(response => {
        this.$swal({
          icon: "success",
          title: "Success",
          text: "Guide Book Uploaded Successfully",
          showConfirmButton: true
        }).then(function () {
          that.step.disabled = false;
          that.getStep(that.$route.params.id)
          that.$refs.guideBook.value = null;
        });
      }
      ).catch((error) => {
        that.step.disabled = false;
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
    downloadToolkit: function (id, name) {
      Api.downloadToolkit(id)
        .then(response => {
          let blob = new Blob([response.data])
          let link = document.createElement('a')
          link.href = window.URL.createObjectURL(blob)
          link.download = name
          link.click()
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
    deleteToolkit: function (id) {
      let that = this
      this.$swal({
        title: 'Are you sure?',
        text: 'You wont be able to revert !',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes Delete it!',
        cancelButtonText: 'No, Keep it!',
        showCloseButton: true,
        showLoaderOnConfirm: true
      }).then((result) => {
        if (result.value) {
          Api.deleteToolkit(id).then(response => {
            this.$swal({
              icon: "success",
              title: "success",
              text: "Deleted Successfully",
              showConfirmButton: true
            }).then(() => {
              that.getToolkitList()
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
    getToolkitList: function () {
      let that = this
      Api.getToolkitList(that.$route.params.id,that.searchData).then(response => {
        that.toolkitList = response.data.res
      });
    },
    getToolkit: function (toolkit) {
      let that = this
        that.$bvModal.show('update-modal')
        that.toolkitUpdate.id = toolkit.id
        that.toolkitUpdate.title = toolkit.title
        that.toolkitUpdate.description = toolkit.description
        // that.toolkitUpdate.file = toolkit.file
    },
    updateToolkit: function (e) {
      e.preventDefault();
      let that = this
      const formData = new FormData();
      formData.append('file', that.toolkitUpdate.file);
      formData.append('id', that.toolkitUpdate.id);
      formData.append('title', that.toolkitUpdate.title);
      formData.append('description', that.toolkitUpdate.description);

      that.toolkitUpdate.disabled = true;
      let headers = {
        'Content-Type': 'multipart/form-data',
        'Access-Control-Allow-Origin': '*'
      }
      Api.updateToolkit(formData, headers).then(response => {
        this.$swal({
          icon: "success",
          title: "Success",
          text: "Toolkit Updated Successfully",
          showConfirmButton: true
        }).then(function () {
          that.toolkitUpdate.disabled = false;
          that.toolkitUpdate.title = ''
          that.toolkitUpdate.description = ''
          that.$refs.fileUpdate.value = null
          that.$bvModal.hide('update-modal')
          that.getToolkitList();
        });
      }
      ).catch((error) => {
        that.toolkit.disabled = false;
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
    this.getStep(this.$route.params.id)
    this.getToolkitList();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
