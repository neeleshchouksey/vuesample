<template>
    <section class="admin-employee-profile-type-section half-cut-bg">
        <h1 class="page-title text-left mt-0">Employee <span>Profile Type</span></h1>
        <div class="row">
            <div class="col-md-3 my-2 d-flex align-items-center">
                <input type="text" v-model="searchData.keyword" class="form-control mb-0 search" placeholder="Search"
                    v-on:keyup="getProfileTypeList" /><span class="search-icon"></span><a href="javascript:void(0)"
                    v-on:click="searchData.keyword = ''; getProfileTypeList()" class="link px-2 ">clear</a>
            </div>
            <div class="col-md-3 my-2">
            </div>
            <div class="col-md-6 d-flex my-2" v-if="user.role == 'ADMIN'">
                <button class="btn btn-primary float-right ml-auto" v-b-modal.add-profile-type-modal>Add Profile
                    Type</button>
            </div>
        </div>
        <div class="table-responsive">
            <table class="table">
                <tr>
                    <th>Profile Type<i class="fa-solid fa-arrow-up"
                            @click="searchData.sortBy = 'profile_type'; searchData.sortOrder='asc';getProfileTypeList()"></i>
                        <i class="fa-solid fa-arrow-down"
                            @click="searchData.sortBy = 'profile_type'; searchData.sortOrder='desc';getProfileTypeList()"></i>
                    </th>
                    <th>File</th>
                    <th>Action</th>
                </tr>
                <tr v-if="profileType.length" v-for="p in profileType" v-bind:key="p.id">
                    <td>{{ p.profile_type }}</td>
                    <td><a class="link" @click="downloadProfileTypeFile(p.id, p.file)">{{ p.file }}</a></td>
                    <td>
                        <div class="d-flex align-items-center p-0" style="min-width: 100px;">
                            <a type="button" class="mx-3 d-block" width="24" @click="getProfileType(p)">
                                <img src="../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
                            </a>
                            <a type="button" class="mx-3 d-block" width="24" @click="deleteProfileType(p.id)">
                                <img src="../../assets/images/table-delete.svg" alt="table-delete" width="24"
                                    height="24" />
                            </a>
                            <router-link type="button" class="mx-3 d-block" width="24"
                                :to="'/admin/employee-profile-type/' + p.id">
                                <img src="../../assets/images/table-eye.svg" alt="table-edit" width="24" height="24" />
                            </router-link>
                        </div>
                    </td>
                </tr>
                <tr v-if="!profileType.length">
                    <td colspan="5">No Data Found</td>
                </tr>
            </table>
        </div>
        <b-modal id="add-profile-type-modal" size="lg" title="Add New Profile Type" :hide-footer=hideFooter no-fade
            no-enforce-focus>
            <form enctype="multipart/form-data" @submit="addProfileTypeF">
                <div class="form-group">
                    <label>Profile Type <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addProfileType.profileType"
                        placeholder="Profile Type" />
                </div>
                <div class="form-group">
                    <label>Upload File <span class="err">*</span></label>
                    <input class="form-control" type="file" ref="file" @change="fileOnChange" />
                </div>
                <div class="form-group">
                    <button type="submit" class="btn btn-primary" :disabled="addProfileType.disabled">Submit</button>
                </div>
            </form>
        </b-modal>
        <b-modal id="update-profile-type-modal" size="lg" title="Update Profile Type" :hide-footer=hideFooter no-fade
            no-enforce-focus>
            <form @submit="updateProfileTypeF" enctype="multipart/form-data">
                <div class="form-group">
                    <label>Title <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateProfileType.profileType"
                        placeholder="Title" />
                </div>
                <div class="form-group">
                    <label>Upload File <span class="err">*</span></label>
                    <input class="form-control" type="file" ref="update_file" @change="updateFileOnChange" />
                </div>
                <div class="form-group">
                    <button type="submit" class="btn btn-primary" :disabled="addProfileType.disabled">Submit</button>
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
    name: 'EmployeeProfileType',
    mixins: [AppMixin],
    data() {
        return {
            hideFooter: true,
            addProfileType: {
                profileType: '',
                file: '',
                disabled: false
            },
            updateProfileType: {
                profileType: '',
                id: '',
                file: '',
                disabled: false
            },
            searchData: {
                'sortBy': '',
                'sortOrder': '',
                'keyword': ''
            }
        }
    },
    components: {
    },
    methods: {
        updateFileOnChange: function (e) {
            let that = this;
            that.updateProfileType.file = ''
            that.updateProfileType.file = this.$refs.update_file.files[0];
        },
        fileOnChange: function (e) {
            let that = this;
            that.addProfileType.file = ''
            that.addProfileType.file = this.$refs.file.files[0];
        },
        addProfileTypeF: function (e) {
            e.preventDefault()
            let that = this;
            if (!that.addProfileType.profileType || !that.addProfileType.file) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required  fields",
                    showConfirmButton: true
                })
            } else {
                that.addProfileType.disabled = true;
                const formData = new FormData();
                formData.append('file', that.addProfileType.file);
                formData.append('profileType', that.addProfileType.profileType);
                Api.addProfileTypeF(formData).then(response => {
                    that.addProfileType.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Profile type added successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.addProfileType.disabled = false
                        that.addProfileType.profileType = ''
                        that.$bvModal.hide('add-profile-type-modal')
                        that.getProfileTypeList()
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.addProfileType.disabled = false;
                    });
                });
            }
        },
        getProfileType: function (data) {
            let that = this;
            that.updateProfileType.id = data.id
            that.updateProfileType.profileType = data.profile_type
            that.$bvModal.show('update-profile-type-modal')
        },
        updateProfileTypeF: function (e) {
            e.preventDefault()
            let that = this;
            if (!that.updateProfileType.profileType) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required  fields",
                    showConfirmButton: true
                })
            } else {
                that.updateProfileType.disabled = true;
                const formData = new FormData();
                formData.append('id', that.updateProfileType.id);
                formData.append('file', that.updateProfileType.file);
                formData.append('profileType', that.updateProfileType.profileType);
                Api.updateProfileTypeF(formData).then(response => {
                    that.updateProfileType.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Profile type updated successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.updateProfileType.disabled = false
                        that.$bvModal.hide('update-profile-type-modal')
                        that.getProfileTypeList()
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.updateProfileType.disabled = false;
                    });
                });
            }
        },
        deleteProfileType: function (id) {
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
                    Api.deleteProfileType(id).then(response => {
                        this.$swal({
                            icon: "success",
                            title: "Success",
                            text: "Profile Type deleted successfully",
                            showConfirmButton: true
                        }).then(function () {
                            that.getProfileTypeList()
                        });
                    }).catch((error) => {
                        this.$swal({
                            icon: "error",
                            title: "error",
                            text: error.response.data.message,
                            showConfirmButton: true
                        }).then(function () {
                        });
                    });
                }
            });
        },

        getProfileTypeList: function () {
            let that = this
            Api.getProfileTypeList1(that.searchData).then(response => {
                that.profileType = response.data.res
            }).catch((error) => {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: error.response.data.message,
                    showConfirmButton: true
                }).then(function () {
                });
            });
        },

    },
    mounted() {
        this.getProfileTypeList()
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
