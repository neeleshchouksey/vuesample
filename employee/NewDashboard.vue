<template>
  <div class="">
    <section class="employer-dashboard pink-pattern-bg">
      <h1 class="page-title text-left mb-0">
        Employee <span class="yellow-bg">Dashboard</span>
      </h1>
      <h2 class="page-sub-title">
        Lorem Ipsum is simply dummy text of the printing and typesetting industry
      </h2>
      <div class="row">
        <div class="col-lg-3 col-md-6 my-2">
          <router-link to="/employee/workshops" class="dashboard-card-link">
            Workshops
            <img src="../../assets/images/chevron-right.svg" alt="arrow-right" />
          </router-link>
        </div>
        <!-- <div class="col-lg-3 col-md-6 my-2">
          <router-link to="/employer/workshops" class="dashboard-card-link">
            Workshops
            <img src="../../assets/images/chevron-right.svg" alt="arrow-right" />
          </router-link>
        </div> -->
        <div class="col-lg-3 col-md-6 my-2">
          <router-link to="/employee/announcements" class="dashboard-card-link">
            Announcements
            <img src="../../assets/images/chevron-right.svg" alt="arrow-right" />
          </router-link>
        </div>
        <!-- <div class="col-lg-3 col-md-6 my-2">
          <router-link to="/employee/resources" class="dashboard-card-link">
            Resources
            <img
              src="../../assets/images/chevron-right.svg"
              alt="arrow-right"
            />
          </router-link>
        </div> -->
      </div>
      <div class="row mt-5 card-row">
        <div class="col-md-6 mb-3">
          <router-link to="/employee/workshops">
            <div class="card">
              <img src="../../assets/images/step-0.png" class="card-img-top" alt="..." />
              <div class="card-body">
                <h6 class="card-sub-title">Step 0</h6>
                <h5 class="card-title-2">
                  Lorem Ipsum is simply dummy text of the printing and
                  typesetting industry
                </h5>
              </div>
            </div>
          </router-link>
        </div>
        <div class="col-md-6 mb-3">
          <router-link to="/employee/workshops">
            <div class="card">
              <img src="../../assets/images/step-2.png" class="card-img-top" alt="..." />
              <div class="card-body">
                <h6 class="card-sub-title">Step 1</h6>
                <h5 class="card-title-2">
                  Lorem Ipsum is simply dummy text of the printing and
                  typesetting industry
                </h5>
              </div>
            </div>
          </router-link>
        </div>
        <div class="col-md-6 mb-3">
          <router-link to="/employee/workshops">
            <div class="card">
              <img src="../../assets/images/step-3.png" class="card-img-top" alt="..." />
              <div class="card-body">
                <h6 class="card-sub-title">Step 2</h6>
                <h5 class="card-title-2">
                  Lorem Ipsum is simply dummy text of the printing and
                  typesetting industry
                </h5>
              </div>
            </div>
          </router-link>
        </div>
        <div class="col-md-6 mb-3">
          <router-link to="/employee/workshops">
            <div class="card">
              <img src="../../assets/images/step-4.png" class="card-img-top" alt="..." />
              <div class="card-body">
                <h6 class="card-sub-title">Step 3</h6>
                <h5 class="card-title-2">
                  Lorem Ipsum is simply dummy text of the printing and
                  typesetting industry
                </h5>
              </div>
            </div>
          </router-link>
        </div>
        <div class="col-md-6 mb-3">
          <router-link to="/employee/workshops">
            <div class="card">
              <img src="../../assets/images/step-5.png" class="card-img-top" alt="..." />
              <div class="card-body">
                <h6 class="card-sub-title">Step 4</h6>
                <h5 class="card-title-2">
                  Lorem Ipsum is simply dummy text of the printing and
                  typesetting industry
                </h5>
              </div>
            </div>
          </router-link>
        </div>

      </div>
    </section>
  </div>
</template>
<script>
/* eslint-disable */
import AppMixin from "../../mixins/AppMixin";

export default {
  name: "NewDashboard",
  mixins: [AppMixin],

  data() {
    return {

    }
  },
};
// var swiper = new Swiper(".mySwiper", {});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.sd-page {
  padding: 0 !important;
}

.sd-root-modern {
  background: transparent !important;
}
</style>
