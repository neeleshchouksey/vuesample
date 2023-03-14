<template>
  <div class="mt-5">
    <div class="row mb-3 align-items-center">
      <div class="col-md-6 my-2 d-flex align-items-center">
        <input type="text" v-model="searchData.keyword" class="form-control search m-0" placeholder="Search By Keyword"
          v-on:keyup="getAnnouncementsList" /><span class="search-icon"></span>
        <a href="javascript:void(0)" v-on:click="searchData.keyword = ''; getAnnouncementsList()"
          class="link px-2 mb-3">clear</a>
      </div>
      <div class="col-md-3 my-2 ">
        <!-- <select v-model="searchData.sortBy" class="form-control m-0" v-on:change="getAnnouncementsList">
          <option value="">Sort By</option>
          <option value="title">Title</option>
          <option value="date">Date</option>
        </select> -->
      </div>
      <div class="col-lg-3 col-md-12 my-2" v-if="company.role == 'COMPANY_ADMIN'">
        <button class="btn btn-primary float-right" v-b-modal.add-announcement-modal>Add Announcement</button>
      </div>
    </div>

    <div class="table-responsive">
      <table class="table">
        <tr>
          <th>Title <i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'title'; searchData.sortOrder='asc';getAnnouncementsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'title'; searchData.sortOrder='desc';getAnnouncementsList()"></i></th>
          <th>Description<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'description'; searchData.sortOrder='asc';getAnnouncementsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'description'; searchData.sortOrder='desc';getAnnouncementsList()"></i></th>
          <th>Date<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'date'; searchData.sortOrder='asc';getAnnouncementsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'date'; searchData.sortOrder='desc';getAnnouncementsList()"></i></th>
          <th v-if="company.role == 'COMPANY_ADMIN'">Action</th>
        </tr>
        <tr v-if="announcementsLength" v-for="a in announcementList.data" v-bind:key="a.id">
          <td>{{ a.title }}</td>
          <td>{{ a.description }}</td>
          <td>{{ a.date | timeStampToDate }}</td>
          <td class="px-0" v-if="company.role == 'COMPANY_ADMIN'">
            <div class="d-flex align-items-center p-0" style="min-width: 100px;">
              <a type="button" class="px-3" @click="getAnnouncement(a.id)" width="24" height="24">
                <img src="../../../assets/images/table-edit.svg" width="24" height="24" alt="table-edit" />
              </a>
              <a type="button" class="px-3" @click="deleteAnnouncement(a.id)" width="24" height="24">
                <img src="../../../assets/images/table-delete.svg" width="24" height="24" alt="table-delete" /></a>
              <span v-if="a.deleted_at">
                Archived
              </span>
            </div>
          </td>
          <!-- <td v-if="company.role == 'COMPANY_ADMIN'">
            <p v-if="!a.deleted_at">
              <button class="btn btn-primary" @click="getAnnouncement(a.id)"><i
                class="fa fa-pencil"></i></button>
              <button class="btn btn-danger" @click="deleteAnnouncement(a.id)"><i
                class="fa fa-trash"></i></button>
            </p>
            <span v-if="a.deleted_at">
                          Archived
                      </span>
          </td> -->
        </tr>
        <tr v-if="!announcementsLength">
          <td colspan="3">No Data Found</td>
        </tr>
      </table>
    </div>
    <pagination :data="announcementList" @pagination-change-page="getAnnouncementsList" />
    <!--Add announcement modal popup-->
    <Add :getAnnouncementsList="getAnnouncementsList"></Add>
    <!--Update announcement modal popup-->
    <Edit :getAnnouncementsList="getAnnouncementsList" :announcementUpdate="announcementUpdate"></Edit>
  </div>
</template>

<script>
/* eslint-disable */
import Api from '../../../router/api'
import AppMixin from '../../../mixins/AppMixin'
import DatePicker from 'vue2-datepicker'
import 'vue2-datepicker/index.css'
import moment from 'moment'
import Add from '../../common/announcements/Add.vue'
import Edit from '../../common/announcements/Edit.vue'

export default {
  name: 'List',
  mixins: [AppMixin],
  components: { DatePicker, Add, Edit },
  data() {
    return {
      hideFooter: true,
      announcementList: {},
      searchData: {
        'sortBy': '',
        'keyword': '',
        'sortOrder':''
      },
      format: true,
      announcementUpdate: {
        'id': '',
        'title': '',
        'description': '',
        'date': '',
        'disabled': false
      },
      announcementsLength: 0,
      disabledDates: {
        to: new Date(Date.now() - 8640000)
      }
    }
  },
  methods: {
    getAnnouncementsList: function (page = 1) {
      let that = this
      Api.getAllAnnouncementsList(that.company.id, that.searchData, page).then(response => {
        let that = this
        that.announcementList = response.data.res
        that.announcementsLength = that.announcementList.data.length
        console.log(that.announcementList.data.length)
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
    deleteAnnouncement: function (id) {
      let that = this
      this.$swal({
        title: 'Are you sure?',
        text: 'You want to archive this announcement !',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes Archive it!',
        cancelButtonText: 'No, Keep it!',
        showCloseButton: true,
        showLoaderOnConfirm: true
      }).then((result) => {
        if (result.value) {
          Api.deleteAnnouncement(id).then(response => {
            this.$swal({
              icon: 'success',
              title: 'success',
              text: 'Deleted Successfully',
              showConfirmButton: true
            }).then(() => {
              that.getAnnouncementsList()
            })
          }
          ).catch((error) => {
            this.$swal({
              icon: 'error',
              title: 'error',
              text: error.response.data.message,
              showConfirmButton: true
            })
          })
        }
      })
    },
    getAnnouncement: function (id) {
      let that = this
      Api.getAnnouncement(id).then(response => {
        that.$bvModal.show('update-announcement-modal')
        that.announcementUpdate.id = response.data.res.id
        that.announcementUpdate.title = response.data.res.title
        that.announcementUpdate.description = response.data.res.description
        that.announcementUpdate.date = new Date(moment.unix(response.data.res.date).format('YYYY-MM-DD HH:mm:ss'))  //moment.unix(response.data.res.date).format('YYYY-MM-DD HH:mm:ss')
        console.log(that.announcementUpdate.date)
      })
    },
  },
  mounted() {
    this.getAnnouncementsList()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
