<template>
  <section class="employee-view-workshop-section pink-pattern-bg">
         <h4 class="page-title text-left"><span>{{ workshopUpdate.title }}</span> </h4>
         <!-- <img :src="workshopPath + '/' + workshopUpdate.img" alt=""> -->
        <div class="card-post">
         <div class="left-image-card d-flex flex-wrap align-items-center">
          <div class="card-image">
            <img  :src="workshopPath + '/' + workshopUpdate.img" alt="images" class="w-100">
          </div>
          <div class="course-details">
            <div class="time w-100">
              <p>
                <span>
                  <img src="../../assets/images/avtar.svg" alt="icon"></span>Time<span>{{workshopUpdate.total_hours}} Hour(s)</span>
                </p>
            </div>
            <div class="place w-100">
              <p>
                <span>
                  <img src="../../assets/images/clock.svg" alt="icon">
                </span>Leader: <span>{{workshopUpdate.instructor}}</span>
              </p>
            </div>
            <div  class="read-more"  >
              <a   v-if="!registered"  class="btn" @click="registerForWorkshop" >Register</a>
              <a v-else class="btn btn-read-more" disabled>Registered</a>
            </div></div>
          </div>
        </div>
         <!--  <div class="team-card-post">
            <div class="team-card-image">
              <div class="course-banner"><img  :src="workshopPath + '/' + workshopUpdate.img" alt="images" class="w-100"></div>
            </div>
            <div class="team-card-content">
              <div class="course-details">
                <h4 class="title">{{ workshopUpdate.title }}</h4>
                <div class="course-middle-details">
                  <div class="time">
                    <p><span><i aria-hidden="true" class="fa fa-clock-o"></i></span>Time: <span>{{
                        workshopUpdate.total_hours
                    }} Hour(s)</span></p>
                  </div>
                  <div class="place">
                    <p> <span><i aria-hidden="true" class="fa fa-user-circle-o"></i></span>Leader: <span>{{
                        workshopUpdate.instructor
                    }}</span></p>
                  </div>
                </div>
                <div class="read-more">
                  <a v-if="!registered" class="btn btn-read-more" @click="registerForWorkshop">Register
                    Now</a>
                  <a v-else class="btn btn-read-more" disabled>Registered</a>
                </div>
              </div>
            </div>
          </div> -->
      <!-- End team section -->
            <h3 class="page-title-2 text-left mb-0">Description</h3>
            <div class="pera-title text-left ">{{ workshopUpdate.description }}</div>
            <h3 class="page-title-2 text-left mb-0">Additional Information</h3>
            <div class="pera-title text-left" v-html="workshopUpdate.additional_info"></div>
            <div class="light-pink-bg py-5 px-3">
            <h2 class="page-title-2 text-left mb-3 pink-color">Meetings</h2>
            <div class="table-responsive">
            <table class="table">
              <tr>
                <th>Topic</th>
                <th>Agenda</th>
                <th>Type</th>
                <th>Date</th>
                <th>Duration</th>
                <th>Passcode</th>
                <th>Visibility</th>
              </tr>
              <tr v-if="workshopMeetings.length" v-for="r in workshopMeetings" v-bind:key="r.id">
                <td>{{ r.topic }}</td>
                <td>{{ r.agenda }}</td>
                <td><span v-if="r.type == 1">Instant</span> <span v-else-if="r.type == 2">Scheduled</span><span
                    v-else-if="r.type == 3">Recurring</span><span v-else>Fixed</span></td>
                <td>{{ r.start_time | timeAgo }}</td>
                <td>{{ r.duration }}</td>
                <td>{{ r.passcode }}</td>
                <td>
                  <!-- <button class="btn btn-primary" @click="getWorkshop(r.id)"><i class="fa fa-pencil"></i></button>
            <button class="btn btn-danger" @click="deleteWorkshop(r.id)"><i class="fa fa-trash"></i></button> -->
                  <a v-if="r.status != 'end'" class="btn btn-primary" target="_blank" :href="r.join_url">Join Meeting</a>
                  <router-link v-if="r.status == 'end'" class="btn btn-primary" :to="'/employee/meeting-recordings/' + r.meeting_id">View Recordings
                  </router-link>
                </td>
              </tr>
              <tr v-if="!workshopMeetings.length">
                <td colspan="5">No Data Found</td>
              </tr>
            </table>
            </div>
            </div>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import CustomSurvey from './CustomSurvey.vue'

export default {
  name: "ViewWorkshop",
  mixins: [AppMixin],
  components: { CustomSurvey },
  data() {
    return {

    }
  },
  methods: {
    registerForWorkshop: function () {
      let that = this
      Api.registerForWorkshop(that.$route.params.id).then(response => {
        this.$swal({
          icon: "success",
          title: "Success",
          text: "You have successfully registered for workshop",
          showConfirmButton: true
        });
        this.getWorkshop(this.$route.params.id)
      })
    }
  },
  created() {
    this.getWorkshop(this.$route.params.id)
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
