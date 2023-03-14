<template>
  <section class="admin-learning-plan-section half-cut-bg">
    <h1 class="page-title text-left mt-0">Workshop<span> Meeting Time</span></h1>
    <div class="mt-5">
      <div class="row mb-3">
        <div class="col-md-3 d-flex my-2 align-items-center">
                <input type="text" v-model="searchData.keyword" class="form-control mb-0 search" placeholder="Search"
                    v-on:keyup="getMeetingsList" /><span class="search-icon"></span><a href="javascript:void(0)"
                    v-on:click="searchData.keyword = ''; getMeetingsList()" class="link px-2 ">clear</a>
            </div>
        <div class="col-md-3">

        </div>
        <div class="col-md-6 d-flex my-2">
          <button class="btn btn-primary float-right ml-auto" v-b-modal.add-modal>Add New Meeting</button>
        </div>
      </div>

      <div class="table-responsive">
        <table class="table">
          <tr>
            <th>Workshop Title<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'title'; searchData.sortOrder='asc';getMeetingsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'title'; searchData.sortOrder='desc';getMeetingsList()"></i></th>
            <th>Topic<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'topic'; searchData.sortOrder='asc';getMeetingsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'topic'; searchData.sortOrder='desc';getMeetingsList()"></i></th>
            <th>Agenda<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'agenda'; searchData.sortOrder='asc';getMeetingsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'agenda'; searchData.sortOrder='desc';getMeetingsList()"></i></th>
            <th>Type</th>
            <th>Date<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'start_time'; searchData.sortOrder='asc';getMeetingsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'start_time'; searchData.sortOrder='desc';getMeetingsList()"></i></th>
            <th>Duration<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'duration'; searchData.sortOrder='asc';getMeetingsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'duration'; searchData.sortOrder='desc';getMeetingsList()"></i></th>
            <th>Passcode</th>
            <th>Action</th>
          </tr>
          <tr v-if="meetingsLength" v-for="r in meetingsList.data" v-bind:key="r.id">
            <td>{{ r.title }}</td>
            <td>{{ r.topic }}</td>
            <td>{{ r.agenda }}</td>
            <td><span v-if="r.type == 1">Instant</span> <span v-else-if="r.type == 2">Scheduled</span><span
                v-else-if="r.type == 3">Recurring</span><span v-else>Fixed</span></td>
            <td>{{ r.start_time | timeAgo }}</td>
            <td>{{ r.duration }}</td>
            <td>{{ r.passcode }}</td>
            <td class="pl-0">
              <div class="d-flex justify-content-end">
                <!-- <button class="btn btn-primary" @click="getWorkshop(r.id)"><i class="fa fa-pencil"></i></button>
            <button class="btn btn-primary m-2" @click="deleteWorkshop(r.id)"><i class="fa fa-trash"></i></button> -->
                <a v-if="r.status != 'end'" class="btn btn-primary m-2" target="_blank" :href="r.start_url">Start
                  Meeting</a>
                <router-link v-if="r.status == 'end'" class="btn btn-primary m-2"
                  :to="'/admin/meeting-recordings/' + r.meeting_id">View Recordings
                </router-link>
                <button v-if="r.status != 'end'" class="btn btn-outline-primary  m-2"
                  @click="endMeeting(r.meeting_id)">End
                  Meeting</button>
                <button v-if="r.status == 'end'" class="btn btn-outline-primary  m-2"
                  @click="sendPostWorkshopSurveyEmail(r.workshop_id)" :disabled="disabled">Send Survey</button>
              </div>
            </td>
          </tr>
          <tr v-if="!meetingsLength">
            <td colspan="5">No Data Found</td>
          </tr>
        </table>

      </div>
    </div>
    <!--Add meeting modal popup-->
    <b-modal id="add-modal" title="Add New Meeting" :hide-footer=hideFooter size="lg" no-fade no-enforce-focus>
      <form>
        <div id="details">
          <div class="form-group">
            <label>Select Workshop <span class="err">*</span></label>
            <select class="form-control" v-model="meeting.workshop_id">
              <option v-for="m in workshopsListSelect" v-bind:key="m.id" :value="m.id">
                {{ m.title }}</option>
            </select>
          </div>
          <div class="form-group">
            <label>Topic <span class="err">*</span></label>
            <input type="text" class="form-control" id="title" placeholder="Topic" v-model="meeting.topic">
          </div>
          <div class="form-group">
            <label>Agenda<span class="err">*</span></label>
            <textarea class="form-control" id="description" placeholder="Agenda" v-model="meeting.agenda"></textarea>
          </div>
          <div class="form-group">
            <label>Select Meeting Type <span class="err">*</span></label>
            <select class="form-control" v-model="meeting.type">
              <option value="1">Instant</option>
              <option value="2">Schedule</option>
              <option value="3">Recurring</option>
              <option value="8">Fixed Recurring</option>
            </select>
          </div>
          <div class="form-group">
            <label>Duration <span class="err">*</span></label>
            <input type="text" class="form-control" id="duration" placeholder="Duration" v-model="meeting.duration">
          </div>
          <div class="form-group">
            <label>Host Video <span class="err">*</span></label>
            <br>
            <input type="radio" id="on" v-model="meeting.host_video" value="1">
            <label for="on" class="mx-2">ON</label>

            <input type="radio" id="off" v-model="meeting.host_video" value="0">
            <label for="off" class="mx-2">OFF</label>
          </div>
          <div class="form-group">
            <label>Participant Video <span class="err">*</span></label>
            <br>
            <input type="radio" id="on1" v-model="meeting.participant_video" value="1">
            <label for="on1" class="mx-2">ON</label>
            <input type="radio" id="off1" v-model="meeting.participant_video" value="0">
            <label for="off1" class="mx-2">OFF</label>
          </div>
          <div class="form-group">
            <label>Date <span class="err">*</span></label>
            <date-picker v-model="meeting.date" type="datetime" :use12h="format" placeholder="Date"
              :disabled-date="disabledRange"></date-picker>
          </div>
          <button type="button" @click="addMeeting" class="btn btn-primary" :disabled="meeting.disabled">Submit</button>
        </div>
      </form>
    </b-modal>
    <!--Update meeting modal popup-->
    <b-modal id="update-modal" title="Update Workshop" :hide-footer=hideFooter size="lg" no-fade no-enforce-focus>
    </b-modal>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import { Vue2TinymceEditor } from "vue2-tinymce-editor";

export default {
  name: 'WorkshopConfiguration',
  mixins: [AppMixin],
  components: {
    Vue2TinymceEditor, DatePicker
  },
  data() {
    return {
      format: true,
      hideFooter: true,
      meeting: {
        'workshop_id': '',
        'topic': '',
        'agenda': '',
        'type': '',
        'date': '',
        'duration': '',
        'participant_video': '',
        'host_video': '',
        'disabled': false,
      },
      disabled: false,
      searchData: {
        'sortBy': '',
        'keyword': '',
        'sortOrder': ''
      },
    }
  },
  methods: {
    addMeeting: function (e) {
      let that = this;
      console.log(that.meeting)
      if (!that.meeting.workshop_id || !that.meeting.topic || !that.meeting.agenda || !that.meeting.duration || !that.meeting.type || !that.meeting.date || that.meeting.host_video == '' || that.meeting.participant_video == '') {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields fields",
          showConfirmButton: true
        });
      } else {
        let that = this
        that.meeting.disabled = true
        Api.addMeeting(that.meeting).then(response => {
          that.meeting.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Meeting created successfully",
            showConfirmButton: true
          }).then(function () {
            that.meeting.workshop_id = ''
            that.meeting.topic = ''
            that.meeting.agenda = ''
            that.meeting.date = ''
            that.meeting.type = ''
            that.meeting.duration = ''
            that.$bvModal.hide('add-modal')
            that.getMeetingsList()
          });
        }
        ).catch((error) => {
          that.meeting.disabled = false;
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.meeting.disabled = false;
          });
        });
      }
    },
    endMeeting: function (id) {
      let that = this
      this.$swal({
        title: 'Are you sure?',
        text: 'You want to end this meeting',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes',
        cancelButtonText: 'No',
        showCloseButton: true,
        showLoaderOnConfirm: true
      }).then((result) => {
        if (result.value) {
          var data = { 'action': 'end' }
          Api.updateMeeting(id, data).then(response => {
            this.$swal({
              icon: "success",
              title: "success",
              text: "Meeting Ended Successfully",
              showConfirmButton: true
            }).then(() => {
              that.getMeetingsList()
            });
          }
          ).catch((error) => {
            this.$swal({
              icon: "error",
              title: "error",
              text: error.response.data.message,
              showConfirmButton: true
            });
          });
        }
      })
    },
    sendPostWorkshopSurveyEmail: function (id) {
      let that = this;
      Api.sendPostWorkshopSurveyEmail(id).then(response => {
        that.disabled = true;
        this.$swal({
          icon: "success",
          title: "Success",
          text: "Post Workshop Survey sent successfully",
          showConfirmButton: true
        }).then(function () {
          that.disabled = false
        });
      }).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        }).then(function () {
          that.disabled = false;
        });
      });
    },
    getMeetingsList: function (page = 1) {
      let that = this
      Api.getMeetingsList(page, that.searchData).then(response => {
        let that = this
        that.meetingsList = response.data.res
        that.meetingsLength = that.meetingsList.data.length
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
    this.getMeetingsList()
    this.getWorkshopsListForSelect()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
