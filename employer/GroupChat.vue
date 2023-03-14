<template>
    <section class="employee-one-to-one-chat-section half-cut-bg pb-0">
      <GroupChatCommon></GroupChatCommon>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import GroupChatCommon from '../common/chat/GroupChat.vue'

export default {
  name: 'GroupChat',
  mixins: [AppMixin],
  components: {GroupChatCommon},
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
