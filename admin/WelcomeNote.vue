<template>
  <section class="admin-welcome-note-section pink-pattern-bg">
    <h1 class="page-title text-left mt-0">Welcome <span>Note </span></h1>
    <div class="row">
      <div class="col-md-4 my-2 d-flex align-items-center">
        <input type="text" v-model="searchData.keyword" class="form-control mb-0 search" placeholder="Search"
          v-on:keyup="getWelcomeNoteList" /><span class="search-icon"></span><a href="javascript:void(0)"
          v-on:click="searchData.keyword = ''; getWelcomeNoteList()" class="link px-2 ">clear</a>
      </div>
      <div class="col-md-2">

      </div>
      <div class="col-md-6 d-flex my-2">
        <button class="btn btn-primary float-right ml-auto" @click="getWelcomeNoteCompanies()" v-b-modal.add-modal>Add
          New Welcome Note</button>
      </div>
    </div>
    <div class="table-responsive">
      <table class="table">
        <tr>
          <th>Image</th>
          <th>Title<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'title'; searchData.sortOrder='asc';getWelcomeNoteList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'title'; searchData.sortOrder='desc';getWelcomeNoteList()"></i></th>
          <th>Description<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'description'; searchData.sortOrder='asc';getWelcomeNoteList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'description'; searchData.sortOrder='desc';getWelcomeNoteList()"></i></th>
          <th>Company</th>
          <th>Action</th>
        </tr>
        <tr v-if="welcome_notes_length" v-for="n in welcome_notes.data" v-bind:key="n.id">
          <td><img v-if="n.image" :src="path + '/' + n.image" class="table-img" height="75" width="75" /><span v-else>NA</span></td>
          <td>{{ n.title }}</td>
          <td v-html="n.description"></td>
          <td><span>{{n.company.map(({company_name})=>company_name).join(',') }}</span></td>
          <td>
            <!-- <button class="btn btn-primary" @click="getCompaniesList(); getSingleWelcomeNote(n.id)"><i
                class="fa fa-pencil"></i></button>
            <button class="btn btn-danger" @click="deleteWelcomeNote(n.id)"><i class="fa fa-trash"></i></button> -->

            <div class="d-flex align-items-center p-0" style="min-width: 100px;">
              <a type="button" class="mx-3 d-block" width="24" @click="getCompaniesList(); getSingleWelcomeNote(n.id)">
                <img src="../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
              </a>
              <a type="button" class="mx-3 d-block" width="24" @click="deleteWelcomeNote(n.id)">
                <img src="../../assets/images/table-delete.svg" alt="table-delete" width="24" height="24" /></a>
            </div>

          </td>
        </tr>
        <tr v-if="!welcome_notes_length">
          <td colspan="5">No Data Found</td>
        </tr>
      </table>
      <pagination :data="welcome_notes" @pagination-change-page="getWelcomeNoteList" />
    </div>

    <b-modal id="add-modal" size="lg" title="Add New Welcome Note" :hide-footer=hideFooter no-fade no-enforce-focus>
      <form @submit="addWelcomeNoteCompany" ref="addWelcomeNoteForm" enctype="multipart/form-data">
        <div class="form-group">
          <label>Select Company <span class="err">*</span></label>
          <multiselect v-model="note.company" :options="companiesList" group-values="values" group-label="selectAll"
            :multiple="true" :group-select="true" placeholder="Type to search" track-by="name" label="name">
            <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
          </multiselect>
        </div>
        <div class="form-group">
          <label>Title <span class="err">*</span></label>
          <input type="text" class="form-control" id="welcome_note" v-model="note.title" placeholder="Title"
            autocomplete="off" />
        </div>
        <div class="form-group">
          <label>Description <span class="err">*</span></label>
          <vue2-tinymce-editor v-model="note.description" placeholder="Description"></vue2-tinymce-editor>
        </div>
        <div class="form-group">
          <label>Upload Image
            <!-- <span class="err">*</span> -->
          </label> <input type="file" ref="welcome_image" id="welcome_image" @change="imageOnChange"
            accept=".jpg, .jpeg, .png" />
        </div>
        <div class="form-group">
          <button type="submit" class="btn btn-primary" :disabled="note.disabled">Add</button>
        </div>
      </form>
    </b-modal>

    <b-modal id="update-modal" size="lg" title="Update Welcome Note" :hide-footer=hideFooter no-fade no-enforce-focus>
      <form @submit="updateWelcomeNoteCompany" ref="updateWelcomeNoteForm" enctype="multipart/form-data">
        <div class="form-group">
          <label>Select Company <span class="err">*</span></label>
          <multiselect v-model="noteUpdate.company" :options="companiesListUpdate" group-values="values"
            group-label="selectAll" :multiple="true" :group-select="true" placeholder="Type to search" track-by="name"
            label="name">
            <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
          </multiselect>
        </div>
        <div class="form-group">
          <label>Title <span class="err">*</span></label>
          <input type="text" class="form-control" id="welcome_note" v-model="noteUpdate.title" placeholder="Title"
            autocomplete="off" />
        </div>
        <div class="form-group">
          <label>Description <span class="err">*</span></label>
          <vue2-tinymce-editor v-model="noteUpdate.description" placeholder="Description"></vue2-tinymce-editor>
        </div>
        <div class="form-group">
          <label>Upload Image
            <!-- <span class="err">*</span> -->
          </label> <input type="file" ref="welcome_image_update" id="welcome_image_update" @change="imageOnChangeUpdate"
            accept=".jpg, .jpeg, .png" />
        </div>

        <div class="form-group">
          <button type="submit" class="btn btn-primary" :disabled="note.disabled">Update</button>
        </div>
      </form>
    </b-modal>

  </section>
</template>
<style src="vue-multiselect/dist/vue-multiselect.min.css">

</style>
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
      hideFooter: true,
      note: {
        'title': '',
        'description': '',
        'image': '',
        'company': '',
        'disabled': false
      },
      noteUpdate: {
        'id': '',
        'title': '',
        'description': '',
        'image': '',
        'company': '',
        'disabled': false
      },
      companiesList: [
        {
          selectAll: 'Select All',
          values: []
        }
      ],
      companiesListUpdate: [
        {
          selectAll: 'Select All',
          values: []
        }
      ],
      welcome_notes: {},
      welcome_note: {},
      path: '',
      welcome_notes_length: 0,
      searchData:{
        'sortBy':'',
        'sortOrder':'',
        'keyword':''
      }
    }
  },
  components: {
    Vue2TinymceEditor
  },
  mixins: [AppMixin],
  methods: {
    imageOnChangeUpdate: function (e) {
      let that = this;
      that.noteUpdate.image = ''
      that.noteUpdate.image = this.$refs.welcome_image_update.files[0];
    },
    imageOnChange: function (e) {
      let that = this;
      that.note.image = ''
      that.note.image = this.$refs.welcome_image.files[0];
    },
    addWelcomeNoteCompany: function (e) {
      e.preventDefault()
      let that = this;
      console.log(that.note.company)
      if (!that.note.company || !that.note.title || !that.note.description) {
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
        formData.append('company', JSON.stringify(that.note.company))
        let headers = {
          'Content-Type': 'multipart/form-data',
          'Access-Control-Allow-Origin': '*'
        }
        Api.addWelcomeNoteCompany(formData).then(response => {
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
            that.$bvModal.hide('add-modal')
            that.getWelcomeNoteList();
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
    getWelcomeNoteCompanies: function (e) {
      let that = this
      Api.getWelcomeNoteCompanies().then(response => {
        that.companiesList[0].values = response.data.res
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        })
      });
    },
    getWelcomeNoteList: function (page = 1) {
      let that = this
      Api.getWelcomeNoteList(page,that.searchData).then(response => {
        that.welcome_notes = response.data.res
        that.welcome_notes_length = that.welcome_notes.data.length
        that.path = response.data.path
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        })
      });
    },
    getSingleWelcomeNote: function (id) {
      let that = this
      Api.getSingleWelcomeNote(id).then(response => {
        that.$bvModal.show('update-modal')
        that.noteUpdate.id = response.data.res.id
        that.noteUpdate.title = response.data.res.title
        that.noteUpdate.description = response.data.res.description
        that.noteUpdate.company = response.data.res.company
      }
      ).catch((error) => {
        this.$swal({
          icon: "error",
          title: "error",
          text: error.response.data.message,
          showConfirmButton: true
        })
      });
    },
    getCompaniesList: function () {
      let that = this
      Api.getCompaniesList().then(response => {
        let that = this
        that.companiesListUpdate[0].values = response.data.res
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
    updateWelcomeNoteCompany: function (e) {
      e.preventDefault()
      let that = this;
      if (!that.noteUpdate.company || !that.noteUpdate.title || !that.noteUpdate.description) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else {
        that.noteUpdate.disabled = true;
        const formData = new FormData();
        formData.append('id', that.noteUpdate.id);
        formData.append('image', that.noteUpdate.image);
        formData.append('title', that.noteUpdate.title);
        formData.append('description', that.noteUpdate.description);
        formData.append('company', JSON.stringify(that.noteUpdate.company))
        let headers = {
          'Content-Type': 'multipart/form-data',
          'Access-Control-Allow-Origin': '*'
        }
        Api.updateWelcomeNoteCompany(formData).then(response => {
          that.noteUpdate.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Welcome note upated successfully",
            showConfirmButton: true
          }).then(function () {
            that.noteUpdate.disabled = false
            that.$refs.welcome_image_update.value = null;
            that.noteUpdate.title = ''
            that.noteUpdate.description = ''
            that.$bvModal.hide('update-modal')
            that.getWelcomeNoteList();
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
    deleteWelcomeNote: function (id) {
      let that = this
      this.$swal({
        title: 'Are you sure?',
        text: 'All the related info will be deleted, you wont be able to revert !',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes Delete it!',
        cancelButtonText: 'No, Keep it!',
        showCloseButton: true,
        showLoaderOnConfirm: true
      }).then((result) => {
        if (result.value) {
          Api.deleteWelcomeNote(id).then(response => {
            this.$swal({
              icon: "success",
              title: "Success",
              text: "Welcome Note deleted successfully",
              showConfirmButton: true
            }).then(function () {
              that.getWelcomeNoteList()
            });
          }).catch((error) => {
            this.$swal({
              icon: "error",
              title: "error",
              text: error.response.data.message,
              showConfirmButton: true
            }).then(function () {
              that.opportunity.disabled = false;
            });
          });
        }
      });
    },

  },
  mounted() {
    this.getWelcomeNoteList()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
