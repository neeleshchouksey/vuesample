<template>
     <section class="opportunities-resources-section half-cut-bg">
      <h1 class="page-title text-left mt-0"><span>Opportunities</span></h1>
        <List></List>
    </section>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import List from '../common/opportunities/List.vue'
export default {
    name: 'Opportunities',
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
