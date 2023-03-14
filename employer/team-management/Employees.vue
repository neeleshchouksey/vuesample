<template>
  <section class="registration-link half-cut-bg">
    <router-link to="/employer/team-management" class="btn back">
      <!-- <button class="btn-primary"> -->
      <img src="../../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
      <!-- </button> -->
    </router-link>
    <h3 class="page-title text-left"><span>Employees</span></h3>
    <div class="row mb-5">
      <div class="col-md-5">
        <div class="row">
          <div class="col-md-6">
            <input type="text" v-model="getEmpData.name" class="form-control" placeholder="Search By Name"
              v-on:keyup="getEmployeesList" />
          </div>
          <div class="col-md-6 d-flex align-items-center">
            <input type="text" v-model="getEmpData.email" class="form-control" placeholder="Search By Email"
              v-on:keyup="getEmployeesList" />
              <a class="link px-2 mb-3"
          href="javascript:void(0)" v-on:click="getEmpData.email = '';getEmpData.name='';getEmployeesList()">clear</a>
          </div>
        </div>
      </div>
    <div class="col-md-4"></div>
      <div class="col-md-3 d-flex align-items-center text-right">
        <a class="links mr-3" v-b-modal.add-employee-modal>+ Add New Employee</a>
        <button class="btn btn-primary float-right" @click="exportEmployees">Export</button>
      </div>
    </div>
    <div class="table-responsive">
      <table class="table">
        <tr>
          <th>Name <i class="fa-solid fa-arrow-up"
              @click="getEmpData.sortBy = 'first_name'; getEmpData.sortOrder='asc';getEmployeesList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getEmpData.sortBy = 'first_name'; getEmpData.sortOrder='desc';getEmployeesList()"></i></th>
          <th>Email <i class="fa-solid fa-arrow-up"
              @click="getEmpData.sortBy = 'email'; getEmpData.sortOrder='asc';getEmployeesList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getEmpData.sortBy = 'email'; getEmpData.sortOrder='desc';getEmployeesList()"></i></th>
          <th>Role <i class="fa-solid fa-arrow-up"
              @click="getEmpData.sortBy = 'role'; getEmpData.sortOrder='asc';getEmployeesList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getEmpData.sortBy = 'role'; getEmpData.sortOrder='desc';getEmployeesList()"></i></th>
          <th>Profile Type <i class="fa-solid fa-arrow-up"
              @click="getEmpData.sortBy = 'profile_type'; getEmpData.sortOrder='asc';getEmployeesList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getEmpData.sortBy = 'profile_type'; getEmpData.sortOrder='desc';getEmployeesList()"></i></th>
          <th>Last Login <i class="fa-solid fa-arrow-up"
              @click="getEmpData.sortBy = 'last_login'; getEmpData.sortOrder='asc';getEmployeesList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="getEmpData.sortBy = 'last_login'; getEmpData.sortOrder='desc';getEmployeesList()"></i></th>
          <th>Action</th>
        </tr>
        <tr v-if="employeesLength" v-for="e in employeesList.data" v-bind:key="e.id">
          <td>{{ e.first_name }} {{ e.last_name }}</td>
          <td>{{ e.email }}</td>
          <td>{{ e.role }}</td>
          <td>{{e.profile_type}}</td>
          <td><span v-if="e.last_login">{{ e.last_login | timeAgo }}</span><span v-else>Not Logged in yet</span></td>
          <td class="px-0">
            <div class="d-flex align-items-center p-0" style="min-width: 100px;">
              <a type="button" class="px-3" @click="getEmployee(e.id)" width="24" height="24">
                <img src="../../../assets/images/table-edit.svg" width="24" height="24" alt="table-edit" />
              </a>
              <a type="button" class="px-3" @click="deleteEmployee(e.id)" width="24" height="24">
                <img src="../../../assets/images/table-delete.svg" width="24" height="24" alt="table-delete" /></a>
            </div>
          </td>
        </tr>
        <tr v-if="!employeesLength">
          <td colspan="5">No Data Found</td>
        </tr>
      </table>
    </div>
    <pagination :data="employeesList" @pagination-change-page="getEmployeesList" />

    <!--Add employee modal popup-->
    <b-modal id="add-employee-modal" title="Add New Employee" :hide-footer=hideFooter>
      <form>
        <div id="details">
          <div class="form-group">
            <label>Email <span class="err">*</span></label>
            <input type="email" class="form-control" id="email" aria-describedby="emailHelp" placeholder="Email"
              v-model="employee.email">
          </div>
          <div class="form-group">
            <label>First Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="firstname" placeholder="First Name" v-model="employee.firstname"
              @keypress="alphabetsOnly">
          </div>
          <div class="form-group">
            <label>Last Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="lastname" placeholder="Last Name" v-model="employee.lastname"
              @keypress="alphabetsOnly">
          </div>
          <div class="form-group">
            <label>Role<span class="err">*</span></label>
            <select v-model="employee.role" class="form-control">
              <option value="">Select Role</option>
              <option value="COMPANY_SUB_ADMIN">Subadmin</option>
              <option value="COMPANY_EMP">Employee</option>
            </select>
          </div>
          <!-- <div class="form-group">
            <label>Profile Type<span class="err">*</span></label>
            <select class="form-control" id="profileType" v-model="employee.profileType">
              <option value="">Select Profile Type</option>
              <option v-for="pt in profileType" :value="pt.id" v-bind:key="pt.id">{{ pt.profile_type }}</option>
            </select>
          </div> -->
          <button type="button" @click="addEmployee" class="btn btn-primary"
            :disabled="employee.disabled">Submit</button>
        </div>
      </form>
    </b-modal>
    <!--Update employee modal popup-->
    <b-modal id="update-employee-modal" title="Update Employee" :hide-footer=hideFooter>
      <form>
        <div id="update-details">
          <div class="form-group">
            <label>First Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="firstname" placeholder="First Name" @keypress="alphabetsOnly"
              v-model="employeeUpdate.firstname">
          </div>
          <div class="form-group">
            <label>Last Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="lastname" placeholder="Last Name" @keypress="alphabetsOnly"
              v-model="employeeUpdate.lastname">
          </div>
          <div class="form-group">
            <label>Role<span class="err">*</span></label>
            <select v-model="employeeUpdate.role" class="form-control">
              <option value="">Select Role</option>
              <option value="COMPANY_SUB_ADMIN">Subadmin</option>
              <option value="COMPANY_EMP">Employee</option>
            </select>
          </div>
          <!-- <div class="form-group">
            <label>Profile Type<span class="err">*</span></label>
            <select class="form-control" id="profileType" v-model="employeeUpdate.profileType">
              <option value="">Select Profile Type</option>
              <option v-for="pt in profileType" :value="pt.id" v-bind:key="pt.id">{{ pt.profile_type }}</option>
            </select>
          </div> -->
          <button type="button" @click="updateEmployee" class="btn btn-primary"
            :disabled="employeeUpdate.disabled">Submit</button>
        </div>
      </form>
    </b-modal>
  </section>
</template>

<script>
/* eslint-disable */
import Api from '../../../router/api'
import AppMixin from '../../../mixins/AppMixin'

export default {
  name: 'Invitations',
  mixins: [AppMixin],
  data() {
    return {
      hideFooter: true,
      employeesList: {},
      employeesLength: 0,
      employee: {
        'link': '',
        'firstname': '',
        'lastname': '',
        'email': '',
        'password': '',
        'disabled': false,
        'role': '',
        'profileType': ''
      },
      employeeUpdate: {
        'id': '',
        'firstname': '',
        'lastname': '',
        'disabled': false,
        'role': '',
        'profileType': ''
      },
      getEmpData: {
        'sortBy': '',
        'name': '',
        'email': '',
        'sortOrder': ''
      }
    }
  },
  methods: {
    exportEmployees: function () {
      let that = this
      Api.exportEmployees(that.company.id, that.getEmpData)
        .then(response => {
          let blob = new Blob([response.data])
          let link = document.createElement('a')
          link.href = window.URL.createObjectURL(blob)
          link.download = 'employees.xlsx'
          link.click()
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

    getEmployeesList: function (page = 1) {
      let that = this
      Api.getEmployeesList(that.company.id, page, that.getEmpData).then(response => {
        let that = this
        that.employeesList = response.data.res
        that.employeesLength = that.employeesList.data.length
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
    addEmployee: function (e) {
      e.preventDefault();
      let that = this;
      if (!that.employee.firstname || !that.employee.lastname || !that.employee.email || !that.employee.role) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else {
        that.employee.disabled = true;
        that.employee.link = that.company.employee_registration_link
        Api.registration(that.employee).then(response => {
          that.employee.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Company employee created successfully",
            showConfirmButton: true
          }).then(function () {
            that.$bvModal.hide('add-employee-modal')
            that.getEmployeesList()
          });
        }
        ).catch((error) => {
          that.employee.disabled = false;
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.employee.disabled = false;
          });
        });
      }
    },
    deleteEmployee: function (id) {
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
          Api.deleteEmployee(id).then(response => {
            this.$swal({
              icon: "success",
              title: "success",
              text: "Deleted Successfully",
              showConfirmButton: true
            }).then(() => {
              that.getEmployeesList()
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
    getEmployee: function (id) {
      let that = this
      Api.getEmployee(id).then(response => {
        that.$bvModal.show('update-employee-modal')
        that.employeeUpdate.id = response.data.res.id
        that.employeeUpdate.firstname = response.data.res.first_name
        that.employeeUpdate.lastname = response.data.res.last_name
        that.employeeUpdate.role = response.data.res.role
        that.employeeUpdate.profileType = response.data.res.profile_type_id
        console.log(that.employeeUpdate.firstname)
      });
    },
    updateEmployee: function () {
      let that = this;
      if (!that.employeeUpdate.firstname || !that.employeeUpdate.lastname || !that.employeeUpdate.role) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else {
        that.employeeUpdate.disabled = true;

        Api.updateEmployee(that.employeeUpdate).then(response => {
          that.employeeUpdate.disabled = false;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Employee details updated successfully",
            showConfirmButton: true
          }).then(function () {
            that.employeeUpdate.disabled = false;
            that.$bvModal.hide('update-employee-modal')
            that.getEmployeesList()
          });
        }
        ).catch((error) => {
          this.$swal({
            icon: "error",
            title: "error",
            text: error.response.data.message,
            showConfirmButton: true
          }).then(function () {
            that.employeeUpdate.disabled = false;
          });
        });
      }
    },
  },
  mounted() {
    this.getEmployeesList()
    this.getProfileTypeList()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
