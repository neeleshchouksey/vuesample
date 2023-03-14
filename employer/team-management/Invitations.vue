<template>
  <section class="registration-link half-cut-bg">
    <router-link to="/employer/team-management" class="btn back">
      <!-- <button class="btn-primary"> -->
        <img src="../../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
      <!-- </button> -->
    </router-link>
     <h3 class="page-title text-left">Invitations</h3>

      <div class="row mb-5">
        <!-- <div class="col-md-3">
          <select v-model="searchData.sortBy" class="form-control" v-on:change="getInvitationsList">
            <option value="">Sort By</option>
            <option value="email">Email</option>
          </select>
        </div> -->
        <div class="col-md-3 my-2 d-flex align-items-center">
          <input type="text" v-model="searchData.email" class="form-control" placeholder="Search By Email"
            v-on:keyup="getInvitationsList" /><a class="link px-2 mb-3"
          href="javascript:void(0)" v-on:click="searchData.email = '';getInvitationsList()">clear</a>
        </div>
      </div>
      <div class="table-responsive">
        <table class="table">
          <tr>
            <th>Email<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'email'; searchData.sortOrder='asc';getInvitationsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'email'; searchData.sortOrder='desc';getInvitationsList()"></i></th>
            <th>Date<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'created_at'; searchData.sortOrder='asc';getInvitationsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'created_at'; searchData.sortOrder='desc';getInvitationsList()"></i></th>
          </tr>
          <tr  v-if="invitationsLength" v-for="e in invitationsList.data" v-bind:key="e.id">
            <td>{{ e.email }}</td>
            <td>{{ e.created_at | timeAgo }}</td>
          </tr>
          <tr v-if="!invitationsLength"><td colspan="2">No Data Found</td></tr>
        </table>
      </div>
      <pagination :data="invitationsList" @pagination-change-page="getInvitationsList" />
  </section>
</template>

<script>
/* eslint-disable */
import Api from '../../../router/api'
import AppMixin from '../../../mixins/AppMixin'

export default {
  name: 'Invitations',
  mixins: [AppMixin],
  data() {
    return {
      hideFooter: true,
      invitationsList: {},
      invitationsLength:0,
      searchData: {
        'sortBy': '',
        'email': '',
        'sortOrder':''
      }
    }
  },
  methods: {
    getInvitationsList: function (page = 1) {
      let that = this
      Api.getInvitationsList(that.company.id, page, that.searchData).then(response => {
        let that = this
        that.invitationsList = response.data.res
        that.invitationsLength = that.invitationsList.data.length
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        });
      });
    },
  },
  mounted() {
    this.getInvitationsList()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
