<template>
  <section class="employee-resources-section half-cut-bg">
      <h1 class="page-title text-left mt-0"><span>Resources</span></h1>
        <List></List>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import List from '../common/resources/List.vue'

export default {
  name: 'Resources',
  mixins: [AppMixin],
  components: { List },
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
