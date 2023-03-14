<template>
  <div class="chat-div">
    <h1 class="page-title text-left">Chat with <span>{{ empChat.first_name }} {{ empChat.last_name }}</span></h1>

    <div v-chat-scroll class="chat-gui" id="chat-gui" ref="scroll_content">
      <p class="text-center" v-if="groupData.limit < total">
        <button class="btn btn-primary load-more" @click="loadMoreMessages">Load More Messages
        </button>
      </p>
      <!-- <div class="message-grp-date"> <span>Today</span></div> -->
      <div class="chat-list" v-if="messagesList.length" v-for="m in messagesList" v-bind:key="m.id"
        v-bind:class="(authUser.emp_id == m.sender_id) ? 'text-right' : ''">
        <p class="d-flex align-items-center">
          <img :src="imagePath + '/profile-images/' + m.profile_image" height="50px" width="50px"
            class="mr-2 border-radius-50" />
          <b>{{ m.first_name }} {{ m.last_name }}</b>
        </p>
        <div class="message-bubble" v-if="m.message_type == 'TEXT'">
          <p v-html="convertToHtml(m.content)"></p>
          <span class="message-time">{{ m.created_at | fromNow }}</span>
        </div>
        <div class="message-bubble" v-if="m.message_type == 'FILE'">
          <!-- <a :href="imagePath + '/chat-attachments/' + m.content" target="_blank">
            <img height="50" width="50" src="../../assets/images/file.png" /><br>
          </a> -->
          <a href="javascript:void(0)" @click="downloadAttachment(m.id, m.content, 'one')">
            <img height="50" width="50" src="../../../assets/images/file.png" /><br><span class="message-time">{{
            m.created_at | fromNow }}</span>
          </a>
        </div>
      </div>
      <div class="chat-list" v-if="!messagesList.length">
        <h4 class="no-msg">Start Messaging</h4>
      </div>
    </div>
    <div>
      <div class="input-group ">
        <input type="text" class="form-control" placeholder="Type here" v-model="dataMsg.message"
          v-on:keyup.enter="sendOneToOneMessage" />
        <button class="send-msg"><img src="../../../assets/images/send-msg.svg" alt="send-msg"
            @click="sendOneToOneMessage" /></button>
        <div class="input-group-append">
          <button class="paper-attachment" type="button" @click="$refs.file.click()">
            <img src="../../../assets/images/paper.svg" alt="icon" />
          </button>
          <input type="file" ref="file" class="d-none" @change="sendOneToOneAttachment">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'

export default {
  name: 'MessageMyTeam',
  mixins: [AppMixin],
  data() {
    return {
      dataMsg: {
        message: '',
        rId: ''
      },
      imageMsg: {
        file: '',
        type: 'one',
        rId: ''
      },

    }
  },
  methods: {
    loadMoreMessages: function () {
      let that = this
      that.groupData.limit = that.groupData.limit + that.groupData.limit
      that.getOneToOneMessage()
    },
    sendOneToOneMessage: function () {
      let that = this
      if (!that.dataMsg.message) {
        this.$swal({
          icon: 'error',
          title: 'error',
          text: 'Please enter message',
          showConfirmButton: true
        })
      } else {
        that.dataMsg.rId = this.$route.params.id
        Api.sendOneToOneMessage(that.dataMsg).then(response => {
          that.dataMsg.message = ''
          that.getOneToOneMessage()
        }
        ).catch((error) => {
          this.$swal({
            icon: 'error',
            title: 'error',
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.anouncements.disabled = false
          })
        })
      }
    },
    sendOneToOneAttachment: function (e) {
      let that = this
      that.imageMsg.rId = this.$route.params.id
      that.imageMsg.file = that.$refs.file.files[0]
      e.preventDefault()
      const formData = new FormData()
      formData.append('file', that.imageMsg.file)
      formData.append('type', that.imageMsg.type)
      formData.append('rId', that.imageMsg.rId)
      let headers = {
        'Content-Type': 'multipart/form-data',
        'Access-Control-Allow-Origin': '*'
      }
      Api.sendAttachment(formData, headers, 'one').then(response => {
        that.getOneToOneMessage()
      }
      ).catch((error) => {
        this.$swal({
          icon: 'error',
          title: 'error',
          text: error.response.data.message,
          showConfirmButton: true
        })
      })
    },
    scrollToEnd() {
      $(document).ready(function () {
        $('.chat-gui').animate({
          scrollTop: $('.chat-gui .chat-list:last-child').position().top
        }, 'slow')
      })
    },

  },
  created() {
    this.getAuthUser()
    this.getEmployeeChat(this.$route.params.id)
    this.getOneToOneMessage()
    if (this.$route.name == 'OneToOneChat') {
      window.Echo.channel('chat' + this.user.id)
        .listen('MessageSent', (e) => {
          console.log(e)
          // this.getOneToOneMessage()
          this.messagesList.push(
            {
              'id':e.messageSent.id,
              'content':e.messageSent.content,
              'message_type':e.messageSent.message_type,
              'first_name':e.first_name,
              'last_name':e.last_name,
              'profile_image':e.profile_image,
              'created_at':e.messageSent.created_at
            })
      });
  }
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
