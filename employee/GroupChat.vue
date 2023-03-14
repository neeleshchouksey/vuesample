<template>
    <section class="employee-one-to-one-chat-section half-cut-bg pb-0">
      <GroupChat></GroupChat>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import GroupChat from '../common/chat/GroupChat.vue'

export default {
  name: 'MessageMyTeam',
  mixins: [AppMixin],
  components: {GroupChat},
  data () {
    return {}
  },
  methods: {},
  created () {

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
