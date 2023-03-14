<template>
  <section class="employer-view-todo-section half-cut-bg">
    <router-link to="/employer/todo" class="btn back">
      <!-- <button class="btn-primary"> -->
      <img src="../../assets/images/arrow-left.svg" alt="arrow-left"/> Back
      <!-- </button> -->
    </router-link>
    <div class="row mt-5">
      <h4 class="page-title text-left mt-0">{{ todoUpdate.title }}</h4>
      <p class="page-sub-title text-left mt-0">{{ todoUpdate.description }}</p>
    </div>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'

export default {
  name: 'ViewTodo',
  mixins: [AppMixin],
  data () {
    return {}
  },
  methods: {},
  mounted () {
    this.getTodo(this.$route.params.id)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
