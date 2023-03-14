<template>
   <section class="admin-todo-section half-cut-bg">
      <h1 class="page-title text-left mt-0"><span>To Do</span></h1>
        <List></List>
    </section>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import List from '../common/todo/List.vue'
export default {
    name: 'Todo',
    mixins: [AppMixin],
    data() {
        return {
         
        }
    },
    components: {
        List
    },
    methods: {
     
    },
    mounted() {
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
