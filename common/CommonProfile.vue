<template>
  <section class="half-cut-bg">
    <div class="col-md-12">
      <div class="row">
        <!-- <div class="col-md-4">
          <div class="profile-sidebar">
            <img class="profile-img" :src="authUser.profile_image" />
            <i class="fa fa-camera update-img-icon" aria-hidden="true" @click="$refs.file.click()"></i>
            <input type="file" ref="file" class="d-none" @change="uploadProfileImage" accept=".jpeg, .jpg, .png">
            <div class="mt-4">
              <h5 class="mt-4">{{ authUser.first_name }} {{ authUser.last_name }}</h5>
              <h6 class="mt-4">{{ authUser.company_name }}</h6>
              <a class="mt-4" :href="authUser.company_domain" target="_blank"><i class="fa fa-globe mr-2"
                  aria-hidden="true"></i>{{ authUser.company_domain }}</a>
            </div>
          </div>
        </div> -->

        <div class="col-md-12" v-if="authUser.role!='ADMIN'">
          <h2 class="page-title text-left my-0  d-inline-block">{{ authUser.first_name }} <span>{{
          authUser.last_name
          }}</span></h2>
          <h2 class="page-sub-title mb-3">{{ authUser.company_name }}</h2>
          <a class=" link mb-5 d-inline-block" :href="authUser.company_domain" target="_blank"><i
              class="fa fa-globe mr-2" aria-hidden="true"></i>{{ authUser.company_domain }}</a>
        </div>
        <div class="col-md-12">
          <!-- <button class="btn-primary float-right" @click="getProfile"><i class="fa fa-pencil"></i></button> -->
          <div class="profile-main">
            <div class="row">
              <div class="col-md-12">
                <div class="profile-sidebar">
                  <img class="profile-img" :src="authUser.profile_image" />
                  <i class="fa fa-camera update-img-icon" aria-hidden="true" @click="$refs.file.click()"></i>
                  <input type="file" ref="file" class="d-none" @change="uploadProfileImage" accept=".jpeg, .jpg, .png">
                </div>
              </div>

              <div class="col-lg-6">
                <div class="profile-details"  v-if="authUser.role!='ADMIN'">
                  <label class="">Full Name</label> <span>{{ authUser.first_name }} {{ authUser.last_name }} </span>
                </div>
                <div class="profile-details">
                  <label class="">Email</label> <span>{{ user.email }}</span>
                </div>
                <div class="profile-details" v-if="authUser.role == 'COMPANY_EMP'">
                  <label class="">Profile Type</label> <span>{{ authUser.profile_type }}</span>
                </div>
                <div class="profile-details border-bottom-0" v-if="authUser.role == 'COMPANY_ADMIN'">
                  <label class="">Total Employees</label> <span>{{ authUser.total_employees }}</span>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="profile-details"  v-if="authUser.role!='ADMIN'">
                  <label class="">Company Name</label> <span>{{ authUser.company_name }}</span>
                </div>
                <div class="profile-details">
                  <label class="">Role</label> <span>{{ authUser.role | replaceUndescore }}</span>
                </div>

                <div class="profile-details" v-if="authUser.role != 'COMPANY_ADMIN' && authUser.title">
                  <label class="">Title</label> <span>{{ authUser.title }}</span>
                </div>
              </div>
              <div class="col-md-12"  v-if="authUser.role!='ADMIN'">
                <a href="#" @click="getProfile" class="btn-primary btn mt-4">
                  Edit Profile
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!--Update profile modal popup-->
    <b-modal id="update-profile-modal" title="Update Profile" :hide-footer=hideFooter>
      <form>
        <div id="update-details">
          <div class="form-group">
            <label>First Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="firstname" placeholder="First Name"
              v-model="profileUpdate.first_name" @keypress="alphabetsOnly">
          </div>
          <div class="form-group">
            <label>Last Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="lastname" placeholder="Last Name"
              v-model="profileUpdate.last_name" @keypress="alphabetsOnly">
          </div>
          <div class="form-group" v-if="authUser.role != 'COMPANY_EMP'">
            <label>Company Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="companyname" placeholder="Company Name"
              v-model="profileUpdate.company_name">
          </div>
          <div class="form-group" v-if="authUser.role != 'COMPANY_EMP'">
            <label>Company Domain <span class="err">*</span></label>
            <input type="text" class="form-control" id="companydomain" placeholder="Company Domain"
              v-model="profileUpdate.company_domain">
          </div>
          <div class="form-group" v-if="authUser.role == 'COMPANY_EMP'">
            <label>Title <span class="err">*</span></label>
            <input type="text" class="form-control" id="title" placeholder="Title" v-model="profileUpdate.title">
          </div>
          <div class="form-group" v-if="authUser.role != 'COMPANY_EMP'">
            <label>Total Employees <span class="err">*</span></label>
            <input type="text" class="form-control" id="title" placeholder="Total Employees" v-model="profileUpdate.total_employees">
          </div>
          <button type="button" @click="updateProfile" class="btn btn-primary" :disabled="profileUpdate.disabled">Update
          </button>
        </div>
      </form>

    </b-modal>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'

export default {
  name: 'CommonProfile',
  mixins: [AppMixin],
  data() {
    return {
      hideFooter: true,
      profile: {
        profile_image: ''
      },
      profileUpdate: {
        'first_name': '',
        'last_name': '',
        'company_name': '',
        'company_domain': '',
        'title': '',
        'disabled': false
      }
    }
  },
  methods: {
    uploadProfileImage: function (e) {
      let that = this
      that.profile.profile_image = that.$refs.file.files[0]
      e.preventDefault()
      const formData = new FormData()
      formData.append('profile_image', that.profile.profile_image)
      let headers = {
        'Content-Type': 'multipart/form-data',
        'Access-Control-Allow-Origin': '*'
      }
      Api.uploadProfileImage(formData, headers).then(response => {
        this.$swal({
          icon: 'success',
          title: 'Success',
          text: 'Profile Image Updated Successfully',
          showConfirmButton: true
        }).then(function () {
          $('.profile-img').attr('src', response.data.res.profile_image)
          $('.profile-image').attr('src', response.data.res.profile_image)
          $('.logo-img').attr('src', response.data.res.profile_image)
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
    },
    updateProfile: function () {
      let that = this
      if (!that.profileUpdate.first_name || !that.profileUpdate.last_name || !that.profileUpdate.company_name) {
        this.$swal({
          icon: 'error',
          title: 'error',
          text: 'Please fill all required fields',
          showConfirmButton: true
        })
      } else {
        that.profileUpdate.disabled = true

        Api.updateProfile(that.profileUpdate).then(response => {
          that.profileUpdate.disabled = false
          this.$swal({
            icon: 'success',
            title: 'Success',
            text: 'Profile details updated successfully',
            showConfirmButton: true
          }).then(function () {
            that.profileUpdate.disabled = false
            that.$bvModal.hide('update-profile-modal')
            that.getAuthUser()
            if (that.authUser.role == 'COMPANY_EMP') {
              $('.c-name').text(that.profileUpdate.title)
            }
            else {
              $('.c-name').text(that.profileUpdate.company_name)
            }
            $('.f-name').text(that.profileUpdate.first_name + ' ' + that.profileUpdate.last_name)

          })
        }
        ).catch((error) => {
          this.$swal({
            icon: 'error',
            title: 'error',
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.profileUpdate.disabled = false
          })
        })
      }
    },
    getProfile: function () {
      this.$bvModal.show('update-profile-modal')
      this.profileUpdate.first_name = this.authUser.first_name
      this.profileUpdate.last_name = this.authUser.last_name
      this.profileUpdate.company_name = this.authUser.company_name
      this.profileUpdate.company_domain = this.authUser.company_domain
      this.profileUpdate.title = this.authUser.title
      this.profileUpdate.total_employees = this.authUser.total_employees
    }
  },
  created() {
    this.getAuthUser()

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
