<template>
    <div class="col-md-9">
        <h3>Request Workshop</h3>
        <List></List>
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
