<template>
    <section class="employee-message-section half-cut-bg pb-0">
    <h1 class="page-title text-left mt-0">Message My <span>Team</span></h1>
    <MessageMyTeamVue></MessageMyTeamVue>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from'../../mixins/AppMixin'
import MessageMyTeamVue from '../common/chat/MessageMyTeam.vue'
export default {
  name: 'MessageMyTeam',
  mixins: [AppMixin],
  components:{MessageMyTeamVue},
  data() {
    return {
      
    }
  },
  methods: {
  },
  created() {
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
