<template>
  <section class="employee-todo-view-section half-cut-bg">
    <div class="container">
      <router-link to="/employee/todo" class="btn back">
        <img src="../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
    </router-link>
        <h4 class="page-title text-left">{{ todoUpdate.title }}</h4>
        <p class="page-sub-title">{{ todoUpdate.description }}</p>
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
  data() {
    return {

    }
  },
  methods: {
  },
  mounted() {
    this.getTodo(this.$route.params.id)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
