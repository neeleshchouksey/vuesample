<template>
<div>
    <section class="employer-announcements-section half-cut-bg">
         <h1 class="page-title text-left mt-0">Request <span>Workshop</span></h1>
        <List></List>
    </section>
</div>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import List from '../common/request-workshops/List.vue'
export default {
    name: 'RequestWorkshop',
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
