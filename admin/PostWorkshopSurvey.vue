<template>
    <section class="admin-learning-plan-section half-cut-bg">
        <h1 class="page-title text-left mt-0">Post Workshop <span>Survey Questions</span></h1>
        <div class="row">
            <div class="col-md-3 d-flex my-2 align-items-center">
                <input type="text" v-model="searchData.keyword" class="form-control mb-0 search" placeholder="Search"
                    v-on:keyup="getPostWorkshopSurveyList" /><span class="search-icon"></span><a href="javascript:void(0)"
                    v-on:click="searchData.keyword = ''; getPostWorkshopSurveyList()" class="link px-2 ">clear</a>
            </div>
            <div class="col-md-3">
            </div>
            <div class="col-md-6 my-2 d-flex" v-if="user.role == 'ADMIN'">
                <button class="btn btn-primary float-right ml-auto" v-b-modal.add-modal>Add Post Workshop
                    Survey</button>
            </div>
        </div>
        <div class="table-responsive">
            <table class="table">
                <tr>
                    <th>Question<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'question'; searchData.sortOrder='asc';getPostWorkshopSurveyList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'question'; searchData.sortOrder='desc';getPostWorkshopSurveyList()"></i></th>
                    <th>Min Rating Description<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'min_desc'; searchData.sortOrder='asc';getPostWorkshopSurveyList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'min_desc'; searchData.sortOrder='desc';getPostWorkshopSurveyList()"></i></th>
                    <th>Max Rating Description<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'max_desc'; searchData.sortOrder='asc';getPostWorkshopSurveyList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'max_desc'; searchData.sortOrder='desc';getPostWorkshopSurveyList()"></i></th>
                    <th>Action</th>
                </tr>
                <tr v-if="postWorkshopSurveyLength" v-for="p in postWorkshopSurvey.data" v-bind:key="p.id">
                    <td>{{ p.question }}</td>
                    <td>{{p.min_desc}}</td>
                    <td>{{p.max_desc}}</td>
                    <td>
                        <div class="d-flex align-items-center p-0" style="min-width: 100px;">
                            <a type="button" class="mx-3 d-block" @click="getPostWorkshopSurvey(p)">
                                <img src="../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
                            </a>
                            <a type="button" class="mx-3 d-block" @click="deletePostWorkshopSurvey(p.id)">
                                <img src="../../assets/images/table-delete.svg" alt="table-edit" width="24"
                                    height="24" />
                            </a>
                            <router-link class="mx-3 d-block" :to="'/admin/post-workshop-survey/' + p.id">
                                <img src="../../assets/images/table-eye.svg" alt="table-edit" width="24" height="24" />
                            </router-link>
                        </div>
                    </td>
                </tr>
                <tr v-if="!postWorkshopSurveyLength">
                    <td colspan="5">No Data Found</td>
                </tr>
            </table>
        </div>
        <b-modal id="add-modal" size="lg" title="Add New Post Workshop Survey" :hide-footer=hideFooter no-fade
            no-enforce-focus>
            <form>
                <div class="form-group">
                    <label>Question <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addData.question" placeholder="Question" />
                </div>
                <div class="form-group">
                    <label>Min Rating Description <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addData.minDesc"
                        placeholder="Min Rating Description" />
                </div>
                <div class="form-group">
                    <label>Max Rating Description <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addData.maxDesc"
                        placeholder="Max Rating Description" />
                </div>
                <div class="form-group">
                    <button type="button" @click="addPostWorkshopSurvey" class="btn btn-primary"
                        :disabled="addData.disabled">Submit</button>
                </div>
            </form>
        </b-modal>
        <b-modal id="update-modal" size="lg" title="Update Post Workshop Survey" :hide-footer=hideFooter no-fade
            no-enforce-focus>
            <form>
                <div class="form-group">
                    <label>Question <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateData.question" placeholder="Question" />
                </div>
                <div class="form-group">
                    <label>Min Rating Description <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateData.minDesc"
                        placeholder="Min Rating Description" />
                </div>
                <div class="form-group">
                    <label>Max Rating Description <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateData.maxDesc"
                        placeholder="Max Rating Description" />
                </div>
                <div class="form-group">
                    <button type="button" @click="updatePostWorkshopSurvey" class="btn btn-primary"
                        :disabled="updateData.disabled">Submit</button>
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
    name: 'PostWorkshopSurveys',
    mixins: [AppMixin],
    data() {
        return {
            hideFooter: true,
            addData: {
                question: '',
                minDesc: '',
                maxDesc: '',
                disabled: false
            },
            updateData: {
                id: '',
                question: '',
                minDesc: '',
                maxDesc: '',
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

        addPostWorkshopSurvey: function (e) {
            let that = this;
            if (!that.addData.question || !that.addData.minDesc || !that.addData.maxDesc) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required  fields",
                    showConfirmButton: true
                })
            } else {
                that.addData.disabled = true;
                Api.addPostWorkshopSurvey(that.addData).then(response => {
                    that.addData.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Post Workshop Survey added successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.addData.disabled = false
                        that.addData.question = ''
                        that.addData.minDesc = ''
                        that.addData.maxDesc = ''
                        that.$bvModal.hide('add-modal')
                        that.getPostWorkshopSurveyList()
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.addData.disabled = false;
                    });
                });
            }
        },
        getPostWorkshopSurvey: function (data) {
            let that = this;
            that.updateData.id = data.id
            that.updateData.question = data.question
            that.updateData.minDesc = data.min_desc
            that.updateData.maxDesc = data.max_desc
            that.$bvModal.show('update-modal')
        },
        updatePostWorkshopSurvey: function (e) {
            let that = this;
            if (!that.updateData.question || !that.updateData.minDesc || !that.updateData.maxDesc) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required  fields",
                    showConfirmButton: true
                })
            } else {
                that.updateData.disabled = true;
                Api.updatePostWorkshopSurvey(that.updateData).then(response => {
                    that.updateData.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Post Workshop Survey updated successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.updateData.disabled = false
                        that.updateData.question = ''
                        that.$bvModal.hide('update-modal')
                        that.getPostWorkshopSurveyList()
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.addData.disabled = false;
                    });
                });
            }
        },
        deletePostWorkshopSurvey: function (id) {
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
                    Api.deletePostWorkshopSurvey(id).then(response => {
                        this.$swal({
                            icon: "success",
                            title: "Success",
                            text: "Post Workshop Survey deleted successfully",
                            showConfirmButton: true
                        }).then(function () {
                            that.getPostWorkshopSurveyList()
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
        getPostWorkshopSurveyList: function () {
            let that = this
            Api.getPostWorkshopSurveyList(that.searchData).then(response => {
                that.postWorkshopSurvey = response.data.res
                that.postWorkshopSurveyLength = that.postWorkshopSurvey.data.length
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
        this.getPostWorkshopSurveyList()
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
