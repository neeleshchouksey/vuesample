<template>
  <div class="sidebar-box">
    <div class="user-info">
      <div class="user-img"><img class="logo-img" :src="authUser.profile_image"></div>
      <div class="user-detail">
        <h5 class="f-name">{{ authUser.first_name }} {{ authUser.last_name }}</h5>
        <p class="mb-0 c-name"> {{ authUser.title ? authUser.title : authUser.company_name }} </p>
      </div>
    </div>
    <div class="siderbar-link">
      <ul>
        <li>
          <router-link to="/employee/dashboard">Home</router-link>
        </li>
        <li>
          <router-link to="/employee/profile">Profile</router-link>
        </li>
        <li>
          <router-link to="/employee/workshops">Workshops</router-link>
        </li>
        <li>
          <router-link to="/employee/announcements">Announcements or Updates</router-link>
        </li>
        <li v-if="authUser.company_assesment_id"><a href="javascript:void(0)"
            @click="assesmentLoginEmployee()">Assessment</a></li>
        <li>
          <router-link to="/employee/resources">Resources</router-link>
        </li>
        <li>
          <router-link to="/employee/todo">To Do</router-link>
        </li>
        <li>
          <router-link to="/employee/welcome-note">Welcome Note</router-link>
        </li>
        <li>
          <router-link to="/employee/feedback-to-company">Feedback to Company
          </router-link>
        </li>
        <li>
          <router-link to="/employee/my-learning-plan">My Learning Plan
          </router-link>
        </li>
        <li>
          <router-link to="/employee/customer-support">Customer Support
          </router-link>
        </li>
        <!-- <li>
          <router-link to="/employee/ask-your-care-team">Ask Your Care Team
          </router-link>
        </li> -->
        <!-- <li>
          <router-link to="/employee/message-my-team">Message My Team</router-link>
        </li> -->
        <li>
          <a href="javascript:void(0)" @click="showGroupList = !showGroupList">Message My Team <i
              class="fa fa-angle-down" aria-hidden="true"></i></a>
        </li>
        <li v-if="showGroupList" class="manage-gap">
          <input type="text" class="form-control search" v-model="groupSearchData.keyword" placeholder="Search Groups"
            @keyup="getChatGroups" /><span class="search-icon"></span>
          <router-link v-for="e in chatGroups.data" v-bind:key="e.id" :to="'/employee/group-chat/' + e.id"
            @click.native="readGroupMessage(e.id)"><img src="../../../assets/images/back-btn.png" alt="btn" />{{
                e.name
            }} <span class="new-message" v-if="e.new_message.length">{{ e.new_message.length }}</span>
          </router-link>
        </li>
        <li>
          <a href="javascript:void(0)" @click="showUserList = !showUserList">One to One Chat <i class="fa fa-angle-down"
              aria-hidden="true"></i>
          </a>
        </li>
        <li v-if="showUserList" class="manage-gap">
          <input type="text" class="form-control search" v-model="searchData.name" placeholder="Search Employees"
            @keyup="getEmployeesListChat" /><span class="search-icon"></span>
          <router-link v-for="e in empList.data" v-bind:key="e.id" :to="'/employee/one-to-one-chat/' + e.id"
            @click.native="readOneToOneMessage(e.id)"><img src="../../../assets/images/back-btn.png" alt="btn" /> {{
                e.first_name
            }} {{ e.last_name }}<span class="new-message" v-if="e.new_message.length">{{ e.new_message.length }}</span>
          </router-link>
        </li>
      </ul>
      <hr />
      <div class="logout-btn-box">
        <button class="logout-btn btn" @click="logout"><img src="../../../assets/images/logout.svg" alt="logout" />
          Logout
        </button>
      </div>
    </div>
  </div>
  <!-- siderbar end -->
</template>
<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
export default {
  name: 'Sidebar',
  data() {
    return {
      path: '',
      chatGroups: []
    }
  },
  mixins: [AppMixin],
  methods: {
    assesmentLoginEmployee: function () {
      var url = ASSESMENT_URL + 'assesment-login-employee?';
      var c = 'id=' + this.user.assesment_id + '&team_assesment_id=' + this.authUser.company_assesment_id + '&email=' + this.user.email + '&name=' + this.company.first_name + ' ' + this.company.last_name + '&password=' + this.user.password;
      window.open(encodeURI(url + c), '_blank')
    }
  },
  created() {
    if (this.isLoggedIn) {
      this.getEmployeesListChat()
      this.getAuthUser()
      this.getChatGroups();
      window.Echo.channel('chat' + this.user.id)
        .listen('MessageSent', (e) => {
          this.getEmployeesListChat()
        });
      window.Echo.channel('group' + this.user.id)
        .listen('GroupMessageSent', (e) => {
          this.getChatGroups()
        });
    }
  },
}
</script>
