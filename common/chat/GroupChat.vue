<template>
  <div class="chat-div">
    <h1 class="page-title text-left">Chat with <span>{{ groupChat.name }} </span></h1>
    <p class="view-group-members"><a href="javascript:void(0)" v-b-modal.group-members-modal>View Group Members</a></p>
    <b-modal id="group-members-modal" :title="'Members in '+ groupChat.name" :hide-footer=hideFooter>
      <div class="form-group">
        <p>{{groupChat.first_name}} {{groupChat.last_name}} (Group Admin)</p>
        <p v-if="groupChat.members" v-for="m in groupChat.members" v-bind:key="m.id">
          {{m.first_name}} {{m.last_name}}
        </p>
      </div>
    </b-modal>

    <div v-chat-scroll class="chat-gui" id="chat-gui" ref="scroll_content">
      <p class="text-center" v-if="groupData.limit < total">
        <button class="btn btn-primary load-more" @click="loadMoreMessages">Load More Messages
        </button>
      </p>
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
          <a href="javascript:void(0)" @click="downloadAttachment(m.id, m.content, 'groupChat')">
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
          v-on:keyup.enter="sendGroupChatMessage" />
        <button class="send-msg"><img src="../../../assets/images/send-msg.svg" alt="send-msg"
            @click="sendGroupChatMessage" /></button>
        <div class="input-group-append">
          <button class="paper-attachment" type="button" @click="$refs.file.click()">
            <img src="../../../assets/images/paper.svg" alt="icon" />
          </button>
          <input type="file" ref="file" class="d-none" @change="sendGroupAttachment">
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
  name: 'GroupChat',
  mixins: [AppMixin],
  data() {
    return {
      hideFooter: false,
      dataMsg: {
        message: '',
        rId: ''
      },
      imageMsg: {
        file: '',
        type: 'groupChat',
        rId: ''
      },
    }
  },
  methods: {
    loadMoreMessages: function () {
      let that = this
      that.groupData.limit = that.groupData.limit + that.groupData.limit
      that.getGroupChatMessage()
    },

    sendGroupChatMessage: function () {
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
        Api.sendGroupChatMessage(that.dataMsg).then(response => {
          that.dataMsg.message = ''
          that.getGroupChatMessage()
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
    sendGroupAttachment: function (e) {
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
      Api.sendAttachment(formData, headers, 'groupChat').then(response => {
        that.getGroupChatMessage()
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
    }
  },
  created() {
    this.getAuthUser()
    this.getChatGroup(this.$route.params.id)
    this.getGroupChatMessage()
    if (this.$route.name == 'GroupChat') {
      window.Echo.channel('group' + this.user.id)
        .listen('GroupMessageSent', (e) => {
          this.messagesList.push(
            {
              'id':e.groupMessageSent.id,
              'content':e.groupMessageSent.content,
              'message_type':e.groupMessageSent.message_type,
              'first_name':e.first_name,
              'last_name':e.last_name,
              'profile_image':e.profile_image,
              'created_at':e.groupMessageSent.created_at
            })
          // this.getGroupChatMessage()
        });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
