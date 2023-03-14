<template>
  <section class="view-step-link half-cut-bg">
    <!--    <router-link to="/employer/dashboard">
      <button class="btn-primary">
        <i class="fa fa-arrow-left white"></i>
      </button>
    </router-link> -->

    <router-link to="/employer/dashboard" class="btn back">
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
          <!-- <div class="row"><h3>Welcome to {{ stepUpdate.title }}</h3></div> -->
          <div class="tab-pane fade show active" id="v-pills-home" role="tabpanel" aria-labelledby="v-pills-home-tab">
            <div class="mb-3">
              <p><b>Overview</b></p>
              {{ stepUpdate.overview }}
            </div>
            <div class="desc-box" v-html="stepUpdate.description"></div>
          </div>
          <div class="tab-pane fade" id="v-pills-profile" role="tabpanel" aria-labelledby="v-pills-profile-tab">
            <div class="row mt-3">
              <div class="row">
                <h5>Uploaded Guide Book</h5>
                <embed v-if="stepUpdate.guideBook" :src="toolkitPath + '/' + stepUpdate.guideBook" width="100%"
                  height="800px" />
                <p v-if="!stepUpdate.guideBook">Guide Book Not uploaded </p>
              </div>
            </div>
          </div>
          <div class="tab-pane fade" id="v-pills-messages" role="tabpanel" aria-labelledby="v-pills-messages-tab">
            <div class="row mt-3">
              <h5 class="page-sub-title mt-5 mb-0">Uploaded Files</h5>
              <div class="row">
                <div class="col-md-4 my-3" v-if="stepUpdate.toolkit.length" v-for="w in stepUpdate.toolkit" v-bind:key="w.id">
                  <div class="card">
                    <div class="card-body">
                      <h5 class="card-title">{{ w.title }}</h5>
                      <p class="card-text">{{ w.description }}</p>
                      <a class="btn btn-read-more" @click="downloadToolkit(w.id, w.file)">Download File</a>
                    </div>
                  </div>
                </div>
              </div>
              <!-- <div class="row mt-3  toolkit-uploaded-files">
                <div class="col-xl-3 col-lg-4 mb-3 col-md-6" v-if="stepUpdate.toolkit.length"
                  v-for="tk in stepUpdate.toolkit" v-bind:key="tk.id">
                  <div class="Uploaded-file-box">
                    <a href="javascript:void(0)" @click="downloadToolkit(tk.id, tk.file)">
                      <i v-if="tk.type == 'png' || tk.type == 'jpg' || tk.type == 'jpeg' || tk.type == 'svg'"
                        class="fa-solid fa-file-image fa-10x"></i>
                      <i v-if="tk.type == 'pdf'" class="fa-solid fa-file-pdf fa-10x"></i>
                      <i v-if="tk.type == 'ppt' || tk.type == 'pptx'" class="fa-solid fa-file-powerpoint fa-10x"></i>
                      <i v-if="tk.type == 'doc' || tk.type == 'docx'" class="fa-solid fa-file-word fa-10x"></i>
                      <i v-if="tk.type == 'csv'" class="fa-solid fa-file-csv fa-10x"></i>
                      <i v-if="tk.type == 'xls' || tk.type == 'xlsx'" class="fa-solid fa-file-excel fa-10x"></i>
                      <br>
                      {{ tk.file | removeTimestampFromFileName }}
                    </a>

                  </div>
                </div>
                <div v-if="!stepUpdate.toolkit.length">
                  No Data Found
                </div>
              </div> -->
            </div>
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
        'file': ''
      }
    }
  },
  methods: {
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
  },
  mounted() {
    this.getStep(this.$route.params.id)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
