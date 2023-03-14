<template>
  <div class="col-md-9 ml-4">
    <div class="row mt-3">
      <div class="col-md-6">
        <h1>CLASSES</h1>
        <p>
          Browse our 100% live online classes led by top instructors and coaches. Choose from our upcoming schedule or
          browse all the classes we offer.
        </p>
      </div>
      <div class="col-md-6">
        <img src="../../assets/images/class-meeting.jpg">
      </div>
    </div>
    <div class="row mt-5" v-for="w in workshopsList" v-bind:key="w.id">
      <div class="col-md-3">
        <img :src="filePath + '/' + w.image" />
      </div>
      <div class="col-md-9">
        <div class="row">
          <div class="col-md-4 mt-4">
            <h1><b>{{ w.title }}</b></h1>
            <span>Time: {{w.total_hours}} Hour(s)  Leader:{{w.instructor}}</span>
            <p>
              {{ w.description }}
            </p>
            <router-link :to="'/employee/workshop/'+w.id" class="btn btn-primary">Read More</router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import CustomSurvey from './CustomSurvey.vue'

export default {
  name: "Workshops",
  mixins: [AppMixin],
  components: { CustomSurvey },
  data() {
    return {
    }
  },
  methods: {

  },
  created() {
    this.getWorkshopsList()
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
