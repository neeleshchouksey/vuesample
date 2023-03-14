<template>
  <div class="mt-5">
    <div class="row mb-3 align-items-center">
      <div class="col-md-3 my-2 d-flex align-items-center">
        <input type="text" v-model="getWorkshopData.keyword" class="form-control mb-0 search" placeholder="Search"
          v-on:keyup="getWorkshopsList" /><span class="search-icon"></span><a href="javascript:void(0)"
          v-on:click="getWorkshopData.keyword = ''; getWorkshopsList()" class="link px-2 ">clear</a>
      </div>
      <div class="col-md-3 my-2">
        <!-- <select v-model="getWorkshopData.sortBy" class="form-control" v-on:change="getWorkshopsList">
          <option value="">Sort By</option>
          <option value="title">Title</option>
        </select> -->
      </div>
      <div class="col-lg-6 col-md-12 my-2 d-flex">
        <button v-if="user.role == 'ADMIN'" class="btn btn-primary ml-auto float-right" v-b-modal.add-workshop-modal>Add
          New
          Workshop</button>
      </div>
    </div>
    <div class="table-responsive">
      <table class="table">
        <tr>
          <th v-if="user.role == 'ADMIN'">Company</th>
          <th>Image</th>
          <th>Title <i class="fa-solid fa-arrow-up"
              @click="getWorkshopData.sortBy = 'title'; getWorkshopData.sortOrder='asc';getWorkshopsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getWorkshopData.sortBy = 'title'; getWorkshopData.sortOrder='desc';getWorkshopsList()"></i></th>
          <th>Description <i class="fa-solid fa-arrow-up"
              @click="getWorkshopData.sortBy = 'description'; getWorkshopData.sortOrder='asc';getWorkshopsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="getWorkshopData.sortBy = 'description'; getWorkshopData.sortOrder='desc';getWorkshopsList()"></i>
          </th>
          <th>Total Hours <i class="fa-solid fa-arrow-up"
              @click="getWorkshopData.sortBy = 'total_hours'; getWorkshopData.sortOrder='asc';getWorkshopsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="getWorkshopData.sortBy = 'total_hours'; getWorkshopData.sortOrder='desc';getWorkshopsList()"></i>
          </th>
          <th>Date <i class="fa-solid fa-arrow-up"
              @click="getWorkshopData.sortBy = 'date'; getWorkshopData.sortOrder='asc';getWorkshopsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getWorkshopData.sortBy = 'date'; getWorkshopData.sortOrder='desc';getWorkshopsList()"></i></th>
          <th>Instructor <i class="fa-solid fa-arrow-up"
              @click="getWorkshopData.sortBy = 'instructor'; getWorkshopData.sortOrder='asc';getWorkshopsList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getWorkshopData.sortBy = 'instructor'; getWorkshopData.sortOrder='desc';getWorkshopsList()"></i>
          </th>
          <th>Meeting Type <i class="fa-solid fa-arrow-up"
              @click="getWorkshopData.sortBy = 'meeting_type'; getWorkshopData.sortOrder='asc';getWorkshopsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="getWorkshopData.sortBy = 'meeting_type'; getWorkshopData.sortOrder='desc';getWorkshopsList()"></i>
          </th>
          <th>Additional Info <i class="fa-solid fa-arrow-up"
              @click="getWorkshopData.sortBy = 'additional_info'; getWorkshopData.sortOrder='asc';getWorkshopsList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="getWorkshopData.sortBy = 'additional_info'; getWorkshopData.sortOrder='desc';getWorkshopsList()"></i>
          </th>
          <th>Action</th>
        </tr>
        <tr v-if="workshopsLength" v-for="r in workshopsList.data" v-bind:key="r.id">
          <td v-if="user.role == 'ADMIN'">
            <!--            <span v-for="c in r.company" v-bind:key="c.id">{{ c.company_name }},</span>-->
            <span>{{r.company.map(({company_name})=>company_name).join(',') }}
            </span>
          </td>
          <td><img :src="filePath + '/' + r.image" class="table-img" height="70" width="70" /></td>
          <td>{{ r.title }}</td>
          <td>{{ r.description }}</td>
          <td>{{ r.total_hours }}</td>
          <td>{{ r.date }}</td>
          <td>{{ r.instructor }}</td>
          <td>{{ r.meeting_type }}</td>
          <td><span v-html="r.additional_info"></span></td>
          <td class="pl-0">

            <div class="d-flex align-items-center p-0" style="min-width: 150px;">
              <a type="button" class="mx-3 d-block" width="24" v-if="user.role == 'ADMIN'"
                @click="getCompaniesListMultiselect(); getWorkshop(r.id)">
                <img src="../../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
              </a>
              <a type="button" class="mx-3 d-block" width="24" v-if="user.role == 'ADMIN'"
                @click="deleteWorkshop(r.id)">
                <img src="../../../assets/images/table-delete.svg" alt="table-delete" width="24" height="24" /></a>

              <router-link v-if="user.role == 'ADMIN'" class="mx-3 d-block" :to="'/admin/view-workshop/' + r.id">
                <img src="../../../assets/images/table-eye.svg" alt="table-eye" width="24" height="24" />
              </router-link>
              <router-link v-if="user.role != 'ADMIN'" class="mx-3 d-block" :to="'/employer/view-workshop/' + r.id">
                <img src="../../../assets/images/table-eye.svg" alt="table-eye" width="24" height="24" />
              </router-link>
            </div>

            <!-- <button v-if="user.role == 'ADMIN'" class="btn btn-primary"
            @click="getCompaniesListMultiselect(); getWorkshop(r.id)"><i class="fa fa-pencil"></i></button>

          <button v-if="user.role == 'ADMIN'" class="btn btn-danger" @click="deleteWorkshop(r.id)"><i
              class="fa fa-trash"></i></button>

          <router-link v-if="user.role == 'ADMIN'" class="btn btn-primary" :to="'/admin/view-workshop/' + r.id"><i
              class="fa fa-eye"></i>
          </router-link>
          <router-link v-if="user.role != 'ADMIN'" class="btn btn-primary" :to="'/employer/view-workshop/' + r.id"><i
              class="fa fa-eye"></i>
          </router-link>-->
          </td>
        </tr>
        <tr v-if="!workshopsLength">
          <td colspan="5">No Data Found</td>
        </tr>
      </table>
    </div>
    <pagination :data="workshopsList" @pagination-change-page="getWorkshopsList" />
    <!--Add workshop modal popup-->
    <Add :getWorkshopsLists="getWorkshopsList"></Add>
    <Edit :getWorkshopsLists="getWorkshopsList" :workshopUpdateData="workshopUpdate"></Edit>
  </div>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import Add from '../workshops/Add'
import Edit from '../workshops/Edit.vue';

export default {
  name: 'List',
  mixins: [AppMixin],
  components: { Add, Edit },
  data() {
    return {
    }
  },
  methods: {
    deleteWorkshop: function (id) {
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
          Api.deleteWorkshop(id).then(response => {
            this.$swal({
              icon: "success",
              title: "success",
              text: "Deleted Successfully",
              showConfirmButton: true
            }).then(() => {
              that.getWorkshopsList()
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
    updateWorkshop: function (e) {
      e.preventDefault()
      let that = this;
      console.log(that.workshopUpdate)
      if (!that.workshopUpdate.title || !that.workshopUpdate.description || !that.workshopUpdate.total_hours || !that.workshopUpdate.instructor || !that.workshopUpdate.date || !that.workshopUpdate.additional_info) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else {
        that.workshopUpdate.disabled = true;
        const formData = new FormData();
        formData.append('id', that.workshopUpdate.id);
        formData.append('image', that.workshopUpdate.image)
        formData.append('title', that.workshopUpdate.title)
        formData.append('description', that.workshopUpdate.description)
        formData.append('total_hours', that.workshopUpdate.total_hours)
        formData.append('instructor', that.workshopUpdate.instructor)
        formData.append('date', that.workshopUpdate.date)
        formData.append('additional_info', that.workshopUpdate.additional_info)
        formData.append('meeting_type', that.workshopUpdate.meeting_type)
        formData.append('company', JSON.stringify(that.workshopUpdate.company))
        let headers = {
          'Content-Type': 'multipart/form-data',
          'Access-Control-Allow-Origin': '*'
        }
        Api.updateWorkshop(formData, headers).then(response => {
          that.workshopUpdate.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Workshop details updated successfully",
            showConfirmButton: true
          }).then(function () {
            that.workshopUpdate.disabled = false;
            that.$bvModal.hide('update-workshop-modal')
            that.getWorkshopsList()
          });
        }).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.workshopUpdate.disabled = false;
          });
        });
      }
    },
  },
  mounted() {
    this.getWorkshopsList()
    this.getCompaniesListMultiselect()
  }
}
</script>
