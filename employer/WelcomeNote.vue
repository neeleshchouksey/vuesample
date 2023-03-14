<template>
  <section class="employee-welcome-note-section mb-5">
    <h2 class="page-title text-left  d-inline-block">Welcome <span>Note</span></h2>
    <!-- <h3 class="page-sub-title pink-color mb-0">Welcome To Mpact</h3> -->
    <div v-if="note">
      <h4 class="section-sub-title">{{ note.title }} </h4>
      <p v-html="note.description"></p>
      <div class="color-border mb-5" v-if="note.image">
        <div class="color-border-box">
          <!-- <iframe  height="480" src="https://www.youtube.com/embed/D0UnqGm_miA" title="Dummy Video For Website" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->
          <img :src="note.image" class="w-100" alt="image" />
        </div>
      </div>
    </div>
    <div v-if="!note">
      Welcome Note not added
    </div>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
export default {
  name: 'WelcomeNote',
  mixins: [AppMixin],
  data() {
    return {
      note: {}
    }
  },
  methods: {
    getSingleWelcomeNoteCompany: function () {
      let that = this
      Api.getSingleWelcomeNoteCompany().then(response => {
        that.note = response.data.res
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        })
      });
    }
  },
  created() {
    this.getSingleWelcomeNoteCompany();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
