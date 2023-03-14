<template>
  <section class="employee-learning-plan-view-section half-cut-bg">
    <div class="row align-items-center">
      <div class="col-md-4 my-3">
        <div class="color-border">
          <img :src="planSingle.image" class="w-100" style="height: 250px;" alt="">
        </div>
      </div>
      <div class="col-md-8 my-3">
        <h2 class="page-title text-left my-0  d-inline-block">{{ planSingle.title }}</h2>
        <p class="page-sub-title mb-0">{{ planSingle.description }}</p>
      </div>
    </div>
    <div class="row">
      <div class="col-md-4 my-3" v-if="planFiles.length" v-for="w in planFiles" v-bind:key="w.id">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{ w.title }}</h5>
            <p class="card-text">{{ w.description }}</p>

            <!--<a class="btn btn-read-more" :href="w.link" target="_blank" v-if="w.link">Watch Video</a>-->
            <a class="btn btn-read-more" @click="showLearningVdo(w.link)" v-if="w.link">Watch Video</a>
            <a class="btn btn-read-more" v-else-if="w.image" @click="downloadLearningPlanFile(w.id, w.image)">Download File</a>
          </div>
        </div>
      </div>  
    </div>

    <!--Video modal popup-->
    <b-modal id="show-learning-vdo" title="Video" :hide-footer=hideFooter>
      <div id="vdo_pop_up" v-html="vdo_pop_up.embed_code" ></div>
    </b-modal>
    <!--End video modal popup-->


  </section>
</template>

<style>
.card-text {
  min-height: 125px !important
}
</style>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'

export default {
  name: 'WelcomeNote',
  mixins: [AppMixin],
  data() {
    return {
      vdo_pop_up: {
        'embed_code': ''
      },
      note: {},
    }
  },
  methods: {
    showLearningVdo: function (embed_code) {
      let that = this
      that.$bvModal.show('show-learning-vdo')
      that.vdo_pop_up.embed_code = embed_code
    },
  },
  created() {
    this.getLearningPlanFiles()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
