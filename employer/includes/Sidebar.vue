<template>
  <div class="sidebar-box">
    <div class="user-info">
      <div class="user-img"><img class="logo-img" :src="company.company_logo"></div>
      <div class="user-detail">
        <h5 class="c-name">{{ company.company_name }}</h5>
        <p class="mb-0 f-name">{{ company.first_name }} {{ company.last_name }} </p>
      </div>
    </div>
    <div class="siderbar-link">
      <ul>
        <li>
          <router-link to="/employer/dashboard">Home</router-link>
        </li>
        <li>
          <router-link to="/employer/employee-dashboard">Employee Dashboard</router-link>
        </li>
        <li>
          <router-link to="/employer/profile">Profile</router-link>
        </li>
        <li>
          <router-link to="/employer/welcome-note">Welcome Note</router-link>
        </li>
        <li>
          <router-link to="/employer/team-management">Team and Organization Management</router-link>
        </li>
        <li>
          <router-link to="/employer/membership-details">Membership Details</router-link>
        </li>
        <li>
          <router-link to="/employer/resources">Resources</router-link>
        </li>
        <li>
          <router-link to="/employer/customer-support">Customer Support</router-link>
        </li>
        <!-- <li>
          <router-link to="/employer/ask-your-care-team">Ask Your Care Team by Employees</router-link>
        </li> -->
        <li>
          <router-link to="/employer/announcements">Announcements or Updates</router-link>
        </li>
        <li>
          <router-link to="/employer/settings">Settings</router-link>
        </li>
        <li><a href="javascript:void(0)" @click="assesmentLogin()">Assessment</a></li>
        <li>
          <router-link to="/employer/request-workshop">Request for Workshop</router-link>
        </li>
        <li>
          <router-link to="/employer/workshops">Workshops</router-link>
        </li>
        <li>
          <router-link to="/employer/feedback-by-employees">Feedback by Employees</router-link>
        </li>
        <li><a href="javascript:void(0)" v-b-modal.create-group-modal>Create New Group <i class="fa fa-plus"
              aria-hidden="true"></i></a></li>
        <li>
          <a href="javascript:void(0)" @click="showGroupList = !showGroupList">Message My Team <i
              class="fa fa-angle-down" aria-hidden="true"></i></a>
        </li>
        <li v-if="showGroupList" class="manage-gap">
          <input type="text" class="form-control search" v-model="groupSearchData.keyword" placeholder="Search Groups"
            @keyup="getChatGroups" /><span class="search-icon"></span>
          <router-link v-for="e in chatGroups.data" v-bind:key="e.id" :to="'/employer/group-chat/' + e.id"  @click.native="readGroupMessage(e.id)"><img
              src="../../../assets/images/back-btn.png" alt="btn" />{{
              e.name
              }}  <span class="new-message" v-if="e.new_message.length">{{e.new_message.length}}</span>
          </router-link>
        </li>
        <li>
          <a href="javascript:void(0)" @click="showUserList = !showUserList">One to One Chat <i class="fa fa-angle-down"
              aria-hidden="true"></i>
          </a>
        </li>
        <li v-if="showUserList" class="manage-gap">
          <input type="text" class="form-control search" v-model="searchData.name" placeholder="Search Employees"
            name="groupname" @keyup="getEmployeesListChat" /><span class="search-icon"></span>
          <router-link v-for="e in empList.data" v-bind:key="e.id" :to="'/employer/one-to-one-chat/' + e.id" @click.native="readOneToOneMessage(e.id)"><img
              src="../../../assets/images/back-btn.png" alt="btn" />
            {{ e.first_name}} {{ e.last_name }} <span class="new-message" v-if="e.new_message.length">{{e.new_message.length}}</span>
          </router-link>
        </li>
      </ul>
      <hr />
      <div class="logout-btn-box"><button class="logout-btn btn" @click="logout"><img
            src="../../../assets/images/logout.svg" alt="logout" />
          Logout
        </button></div>
    </div>
    <b-modal id="create-group-modal" title="Create New Group" :hide-footer=hideFooter>

      <div class="form-group">
        <label>Add Employees</label>

        <input type="text" class="form-control search" v-model="searchData.name"
          placeholder="Search Employees to Add in Group" @keyup="getEmployeesListChat" />
        <span class="search-icon"></span>
        <p class="mb-0" v-for="e in empList.data" v-bind:key="e.id">
          <input type="checkbox" v-model="createGroup.user" :id="e.id" :value="e.id" />
          <label>{{ e.first_name }} {{ e.last_name }}</label>
        </p>
      </div>
      <div class="form-group">
        <label>Enter Group Name</label>
        <input type="text" class="form-control" v-model="createGroup.name" placeholder="Enter Group Name" />
      </div>
      <button type="button" class="btn btn-primary" @click="createChatGroup" :disabled="createGroup.disabled">Create
        Group</button>

    </b-modal>
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
      hideFooter: true,
      createGroup: {
        user: [],
        name: '',
        disabled: false
      },
      chatGroups: []
    }
  },
  mixins: [AppMixin],
  methods: {
    createChatGroup: function () {
      let that = this;
      if (!that.createGroup.name) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please enter group name",
          showConfirmButton: true
        })
      } else if (!that.createGroup.user.length) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please select atleast one group member",
          showConfirmButton: true
        })
      } else {
        Api.createChatGroup(that.createGroup).then(response => {
          that.createGroup.disabled = false
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Group created successfully",
            showConfirmButton: true
          }).then(function () {
            that.createGroup.disabled = false
            that.createGroup.user = []
            that.createGroup.name = ''
            that.$bvModal.hide('create-group-modal')
            that.getChatGroups();
          });
        }).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.createGroup.disabled = false
          });
        });
      }
    },
    
  },
  created() {
    if (this.isLoggedIn) {
      this.getEmployeesListChat()
      this.getAuthUser();
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
