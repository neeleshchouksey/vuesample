<template>
  <div>
    <div class="employee-workshops-section pink-pattern-bg">
      <div class="container">
        <section class="live-classe">
          <div class="container">
            <div class="row">
              <div class="col-md-12">
                <h2 class="page-title mb-0 text-left">
                  <span></span>
                </h2>
                <h3 class="page-sub-title">
                  Browse our 100% live online classes led by top instructors and
                  coaches. Choose from our upcoming schedule or browse all the
                  classes we offer.
                </h3>
              </div>
              <div class="col-md-12">
                <div class="classes-seminar  color-border">
                  <img
                    src="../../assets/images/class-meeting.jpg"
                    class="w-100"
                    alt="meeting img"
                  />
                </div>
              </div>
            </div>
          </div>
        </section>

        <section class="clases-blogs">
          <!-- <div class="row row-mb" v-if="workshopsLength" v-for="w in workshopsList.data" v-bind:key="w.id">
          <div class="col-md-4">
            <div class="course-banner"><img :src="filePath + '/' + w.image" alt=""></div>
          </div>
          <div class="col-md-8">
            <div class="course-details">
              <h4 class="title">{{ w.title }}</h4>
              <div class="course-middle-details">
                <div class="time">
                  <p>Time: <span>{{ w.total_hours }} Hour(s)</span></p>
                </div>
                <div class="place">
                  <p>Leader: <span>{{ w.instructor }}</span></p>
                </div>
              </div>
              <div class="course-description">
                <p>{{ w.description }}</p>
              </div>
              <div class="read-more">
                <router-link :to="'/employee/workshop/' + w.id" class="btn btn-read-more">Read More</router-link>
              </div>
            </div>
          </div>
        </div>
        <pagination :data="workshopsList" @pagination-change-page="getWorkshopsList" />
        <div class="row row-mb" v-if="!workshopsLength">
          No Workshops Found
        </div> -->
          <div
            class="card-post"
            v-if="workshopsLength"
            v-for="w in workshopsList.data"
            v-bind:key="w.id"
          >
            <h4 class="section-title mb-0">{{ w.title }}</h4>
            <h5 class="page-sub-title mb-4">{{ w.description }}</h5>
            <div class="left-image-card d-flex flex-wrap align-items-center">
              <div class="card-image">
                <img
                  :src="filePath + '/' + w.image"
                  class="w-100"
                  alt="images"
                />
              </div>
              <div class="course-details">
                <div class="time w-100">
                  <p>
                    <span
                      ><img
                        src="../../assets/images/clock.svg"
                        alt="icon"
                      /> </span
                    >Time<span>{{ w.total_hours }} Hour(s)</span>
                  </p>
                </div>
                <div class="place w-100">
                  <p>
                    <span
                      ><img
                        src="../../assets/images/avtar.svg"
                        alt="icon"/></span
                    >Leader: <span>{{ w.instructor }}</span>
                  </p>
                </div>
                <div class="place w-100">
                  <p>
                    <span
                      ><img
                        src="../../assets/images/Calendar-icon.svg"
                        alt="workshops image"
                      />Date: Date: {{ w.date | timeStampToDateOnly }} Time:
                      {{ w.date | timeStampToTimeOnly }}</span
                    >
                  </p>
                </div>
                <div class="read-more">
                  <router-link :to="'/employee/workshop/' + w.id" class="btn"
                    >Read More</router-link
                  >
                  <button class="btn" v-if="w.registered">Registered</button>
                  <a
                    v-if="!w.registered"
                    class="btn"
                    @click="registerForWorkshop(w.id)"
                    >Register</a
                  >
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import AppMixin from "../../mixins/AppMixin";
import Api from "../../router/api";
import CustomSurvey from "./CustomSurvey.vue";

export default {
  name: "Workshops",
  mixins: [AppMixin],
  components: { CustomSurvey },
  data() {
    return {};
  },
  methods: {
    registerForWorkshop: function(id) {
      let that = this;
      Api.registerForWorkshop(id).then(response => {
        this.$swal({
          icon: "success",
          title: "Success",
          text: "You have successfully registered for workshop",
          showConfirmButton: true
        });
        this.getWorkshopsList();
      });
    }
  },
  created() {
    this.getWorkshopsList();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
