c<template>
  <section class="admin-workshops-section half-cut-bg">
      <h1 class="page-title text-left mt-0">Workshops</h1>
    <div class="mt-5">
      <div class="row mb-3 align-items-center">
        <div class="col-md-4">
          <div class="color-border">
            <img :src="workshopPath + '/' + workshopUpdate.img" style="height: 250px;" />
          </div>
        </div>
        <div class="col-md-4">
              <h2 class="section-title text-left my-0  d-inline-block">{{ workshopUpdate.title }}</h2>
              <p class="page-sub-title mb-0">Time: {{ workshopUpdate.total_hours }} Hour(s) Leader:{{ workshopUpdate.instructor }}</p>
        </div>
      </div>
      <div class="row mt-3">
        <h3 class="page-title text-left mb-3">Description</h3>
        <p class="page-sub-title mb-3">{{ workshopUpdate.description }}</p>
      </div>
      <div class="row mt-3" v-html="workshopUpdate.additional_info">
      </div>
    </div>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import { Vue2TinymceEditor } from "vue2-tinymce-editor";

export default {
  name: 'WorkshopConfiguration',
  mixins: [AppMixin],
  components: {
    Vue2TinymceEditor, DatePicker
  },
  data() {
    return {

    }
  },
  methods: {

  },
  mounted() {
    this.getWorkshop(this.$route.params.id)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
