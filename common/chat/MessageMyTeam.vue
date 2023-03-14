<template>
  <div class="chat-div">
    <div v-chat-scroll class="chat-gui" id="chat-gui" ref="scroll_content">
      <p class="text-center" v-if="groupData.limit < total">
        <button class="btn btn-primary load-more"
                @click="loadMoreMessages">Load More Messages
        </button>
      </p>
<!-- <div class="message-grp-date"> <span>Today</span></div> -->

<div class="chat-list" v-if="messagesList.length" v-bind:class="(authUser.emp_id == m.sender_user_id) ? 'text-right' : ''" v-for="m in messagesList" v-bind:key="m.id">
        <p class="d-flex align-items-center">
          <img class="mr-2 border-radius-50" :src="imagePath + '/profile-images/' + m.profile_image" height="50px"
               width="50px"/>
          <b>{{ m.first_name }} {{ m.last_name }}</b>
        </p>
        <div class="message-bubble" v-if="m.message_type == 'TEXT'">
          <p v-html="convertToHtml(m.content)">
          </p>
          <span class="message-time">{{ m.created_at | fromNow }}</span>
        </div>
        <div class="message-bubble" v-if="m.message_type == 'FILE'">
          <!-- <a :href="imagePath + '/chat-attachments/' + m.content" target="_blank">
            <img height="50" width="50" src="../../assets/images/file.png" /><br>
          </a> -->

          <a href="javascript:void(0)" @click="downloadAttachment(m.id, m.content, 'group')">
            <img height="50" width="50" src="../../../assets/images/file.png"/><br><span
            class="message-time">{{ m.created_at | fromNow }}</span>
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
               v-on:keyup.enter="sendGroupMessage"/>
        <button class="send-msg"><img src="../../../assets/images/send-msg.svg" alt="send-msg" @click="sendGroupMessage"/></button>
        <div class="input-group-append">
          <button class="paper-attachment" type="button" @click="$refs.file.click()">
            <img src="../../../assets/images/paper.svg" alt="icon"/>
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
import axios from 'axios'

export default {
  name: 'MessageMyTeam',
  mixins: [AppMixin],
  data () {
    return {
      messagesList: [],
      dataMsg: {
        message: ''
      },
      imageMsg: {
        file: '',
        type: 'group'
      },
      imagePath: '',
      groupData: {
        'limit': 10,
        'offset': 0
      },
      total: 0
    }
  },
  methods: {
    loadMoreMessages: function () {
      let that = this
      that.groupData.limit = that.groupData.limit + that.groupData.limit
      that.getGroupMessage()
    },
    getGroupMessage: function () {
      let that = this
      Api.getGroupMessage(that.groupData).then(response => {
          that.messagesList = response.data.res
          that.imagePath = response.data.path
          that.total = response.data.total
          this.scrollToBottom()
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
    sendGroupMessage: function () {
      let that = this
      if (!that.dataMsg.message) {
        this.$swal({
          icon: 'error',
          title: 'error',
          text: 'Please enter message',
          showConfirmButton: true
        })
      } else {
        Api.sendGroupMessage(that.dataMsg).then(response => {
            that.dataMsg.message = ''
            that.getGroupMessage()
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
      that.imageMsg.file = that.$refs.file.files[0]
      e.preventDefault()
      const formData = new FormData()
      formData.append('file', that.imageMsg.file)
      formData.append('type', that.imageMsg.type)
      let headers = {
        'Content-Type': 'multipart/form-data',
        'Access-Control-Allow-Origin': '*'
      }
      Api.sendAttachment(formData, headers).then(response => {
          that.getGroupMessage()
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
  },
  created () {
    this.getAuthUser()
    this.getGroupMessage()
    console.log('chat' + this.user.id)
    window.Echo.channel('chat' + this.user.id)
      .listen('MessageSent', (e) => {
        console.log('Event calling' + 'MessageSent' + this.user.id)
        this.getGroupMessage()
      })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
