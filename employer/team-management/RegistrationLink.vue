<template>
  <section class="registration-link half-cut-bg">
    <router-link to="/employer/team-management" class="btn back">
      <!-- <button class="btn-primary"> -->
        <img src="../../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
      <!-- </button> -->
    </router-link>
      <h3 class="page-title mb-0 text-left">Employee <br><span>Registration Link</span> </h3>
      <h5 class="page-sub-title word-break-all">{{ employerDetails.employee_registration_link }}</h5>

      <button class="btn btn-primary mr-3" v-b-modal.email-link-modal>Send Link to Email</button>
      <button class="btn btn-outline-primary" type="button" v-clipboard:copy="employerDetails.employee_registration_link"
        v-clipboard:success="onCopy">
        Copy Link
      </button>
      <p v-if="linkCopied" class="success">
        Link Copied
      </p>

    <!--Send registration link to email popup-->
    <b-modal id="email-link-modal" title="Send link to email" :hide-footer=hideFooter class=" half-cut-bg">
      <div class="d-flex align-items-center flex-wrap" style="min-height: 40vh;">
      <div class="form-group w-100">
        <label>Enter Email <span class="err">*</span></label>
        <input type="email" class="form-control" id="forgot-email" aria-describedby="emailHelp" placeholder="Email"
          v-model="sendEmail.email">
      </div>
      <button type="button" class="btn btn-primary" @click="sendLinkToEmail" :disabled="sendEmail.disabled">Send
        Email</button>
        </div>
    </b-modal>
  </section>
</template>

<script>
/* eslint-disable */
import Api from '../../../router/api'
import AppMixin from '../../../mixins/AppMixin'

export default {
  name: 'RegistrationLink',
  data() {
    return {
      hideFooter: true,
      sendEmail: {
        'email': '',
        'link': '',
        'company_name': '',
        'company_id': '',
        'disabled': false
      },
      linkCopied: false
    }
  },
  mixins: [AppMixin],
  methods: {
    sendLinkToEmail: function () {
      let that = this
      if (!that.sendEmail.email) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please Enter Email",
          showConfirmButton: true
        });
      } else {
        that.sendEmail.link = that.employerDetails.employee_registration_link
        that.sendEmail.company_name = that.employerDetails.company_name
        that.sendEmail.company_id = that.employerDetails.id
        that.sendEmail.disabled = true
        Api.sendLinkToEmail(that.sendEmail).then(response => {
          this.$swal({
            icon: "success",
            title: "success",
            text: "Email sent successfully",
            showConfirmButton: true
          }).then(() => {
            that.sendEmail.email = ''
            that.sendEmail.disabled = false
            that.$bvModal.hide('email-link-modal')
          });
        }
        ).catch((error) => {
          that.sendEmail.disabled = false
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          });
        });
      }
    },
    onCopy: function (e) {
      // alert('You just copied the following text to the clipboard: ' + e.text)
      this.linkCopied = true
      setTimeout(() => this.linkCopied = false, 2000);
    },
  },
  mounted() {
    this.getEmployeeRegistrationLink()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
