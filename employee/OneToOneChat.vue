<template>
    <section class="employee-one-to-one-chat-section half-cut-bg pb-0">
      <OneToOneChat></OneToOneChat>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import OneToOneChat from '../common/chat/OneToOneChat.vue'

export default {
  name: 'GroupChat',
  mixins: [AppMixin],
  components: {OneToOneChat},
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
