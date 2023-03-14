<template>
<section class="employer-dashboard  pettern-bg">
    <h1 class="page-title text-left"><span>Settings</span></h1>
    <div class="row">
      <div class="col-lg-6">
        <form @submit="addWelcomeNote" ref="addWelcomeNoteForm" enctype="multipart/form-data">
          <div class="form-group">
            <label><b>Welcome</b></label><br>
            <label>Title <span class="err">*</span></label>
            <input type="text" class="form-control" id="welcome_note" v-model="note.title" placeholder="Title"
              autocomplete="off" />
          </div>
          <div class="form-group">
            <label>Description <span class="err">*</span></label>
            <vue2-tinymce-editor v-model="note.description" placeholder="Description"></vue2-tinymce-editor>
          </div>

          <div class="row align-items-center">
            <div class="col-md-6 my-2">
              <div class="form-group">
                <label>Upload Image <span class="err">*</span></label> <input type="file" class="form-control" ref="welcome_image" id="welcome_image"
                  @change="imageOnChange" accept=".jpg, .jpeg, .png" />
              </div>
            </div>
            <div class="col-md-3 my-2">
              <div class="form-group" v-if="note.oldImage">
                <label>Uploaded Image</label>
                <img :src="note.oldImage" height="70" width="70" />
              </div>
            </div>
            <div class="col-md-3 my-2">
              <div class="form-group mt-3 mb-0">
                <button type="submit" class="btn btn-outline-primary" :disabled="note.disabled">Update</button>
              </div>
            </div>
          </div>
        </form>
      </div>
      <div class="col-lg-6">
        <form @submit="uploadLogo" ref="createCompanyForm" enctype="multipart/form-data">
          <div class="row align-items-center">
            <div class="col-md-9 my-2">
              <div class="form-group">
                <label><b>Change Logo </b></label>
                <br>
                <label>Upload your logo <span class="err">*</span></label>
                <input type="file" class="form-control" ref="logo" id="logo" @change="fileOnChange"
                  accept=".jpg, .jpeg, .png" />
              </div>
            </div>
            <div class="col-md-3 my-2">
              <div class="form-group mt-3 mb-0">
                <button type="submit" class="btn btn-outline-primary" :disabled="upload.disabled">Upload</button>
              </div>
            </div>
          </div>
        </form>

        <label><b>Change Password</b></label>
        <div class="form-group">
          <label>Enter Old Password <span class="err">*</span></label>
          <input type="password" class="form-control" id="password" placeholder="Old Password"
            v-model="changePass.oldPassword">
        </div>
        <div class="form-group">
          <label>Enter New Password <span class="err">*</span></label>
          <input type="password" class="form-control" id="password" placeholder="New Password"
            v-model="changePass.newPassword">
        </div>
        <div class="form-group">
          <label>Retype New Password <span class="err">*</span></label>
          <input type="password" class="form-control" id="cpassword" placeholder="New Password"
            v-model="changePass.confirmNewPassword">
        </div>
        <div class="form-group">
          <button type="button" class="btn btn-primary" :disabled="changePass.disabled"
            @click="changePassword">Change</button>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import axios from 'axios'
import Api from '../../router/api'
import { Vue2TinymceEditor } from "vue2-tinymce-editor";

export default {
  name: 'Settings',
  data() {
    return {
      upload: {
        'logo': '',
        'disabled': false
      },
      changePass: {
        'oldPassword': '',
        'newPassword': '',
        'confirmNewPassword': '',
        'disabled': false
      }
    }
  },
  components: {
    Vue2TinymceEditor
  },
  mixins: [AppMixin],
  methods: {
    fileOnChange: function (e) {
      let that = this;
      this.upload.logo = this.$refs.logo.files[0];
    },
    uploadLogo: function (e) {
      e.preventDefault();
      let that = this
      const formData = new FormData();
      formData.append('logo', that.upload.logo);
      that.upload.disabled = true;
      let headers = {
        'Content-Type': 'multipart/form-data',
        'Access-Control-Allow-Origin': '*'
      }
      Api.uploadLogo(formData, headers).then(response => {
        this.$swal({
          icon: "success",
          title: "Success",
          text: "Logo Changed Successfully",
          showConfirmButton: true
        }).then(function () {
          that.upload.disabled = false;
          that.$refs.logo.value = null;
          $('.logo-img').attr('src', response.data.res.company_logo)
        });
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        }).then(function () {
          that.upload.disabled = false;
        });
      });
    },
    imageOnChange: function (e) {
      let that = this;
      that.note.image = ''
      that.note.image = this.$refs.welcome_image.files[0];
    },
    addWelcomeNote: function (e) {
      e.preventDefault()
      let that = this;
      if (!that.note.title || !that.note.description || !that.note.image) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else {
        that.note.disabled = true;
        const formData = new FormData();
        formData.append('image', that.note.image);
        formData.append('title', that.note.title);
        formData.append('description', that.note.description);

        let headers = {
          'Content-Type': 'multipart/form-data',
          'Access-Control-Allow-Origin': '*'
        }
        Api.addWelcomeNote(formData).then(response => {
          that.note.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Welcome note added successfully",
            showConfirmButton: true
          }).then(function () {
            that.note.disabled = false
            that.$refs.welcome_image.value = null;
            that.note.title = ''
            that.note.description = ''
            that.getWelcomeNote();
          });
        }
        ).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.note.disabled = false;
          });
        });
      }
    },

    changePassword: function () {
      let that = this;
      if (!that.changePass.oldPassword || !that.changePass.newPassword || !that.changePass.confirmNewPassword) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else if (that.changePass.newPassword != that.changePass.confirmNewPassword) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "New password and Confirm password not matched",
          showConfirmButton: true
        });
      } else {
        that.changePass.disabled = true;
        Api.changePassword(that.changePass).then(response => {
          that.changePass.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Password changed successfully",
            showConfirmButton: true
          }).then(function () {
            that.changePass.disabled = false
            that.changePass.oldPassword = ''
            that.changePass.newPassword = ''
            that.changePass.confirmNewPassword = ''
          });
        }).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.changePass.disabled = false;
          });
        });
      }
    },
  },
  mounted() {
    this.getWelcomeNote()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
