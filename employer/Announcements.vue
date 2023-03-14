<template>
<div>
  <section class="employer-announcements-section half-cut-bg">
    <h1 class="page-title text-left mt-0"><span>Announcements</span></h1>
    <List></List>
  </section>
</div>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import List from '../common/announcements/List.vue'

export default {
  name: 'AnnouncementList',
  mixins: [AppMixin],
  components:{List},
  data() {
    return {
   
    }
  },
  methods: {

  },
  mounted() {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
