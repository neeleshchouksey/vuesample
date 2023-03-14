<template>
    <section class="admin-learning-plan-section half-cut-bg">
        <h1 class="page-title text-left mt-0">Popup Survey <span>Questions</span></h1>
        <div class="row">
            <div class="col-md-3 d-flex my-2 align-items-center">
                <input type="text" v-model="searchData.keyword" class="form-control mb-0 search" placeholder="Search"
                    v-on:keyup="getPopupSurveyList" /><span class="search-icon"></span><a href="javascript:void(0)"
                    v-on:click="searchData.keyword = ''; getPopupSurveyList()" class="link px-2 ">clear</a>
            </div>
            <div class="col-md-3">
            </div>
            <div class="col-md-6 my-2 d-flex" v-if="user.role == 'ADMIN'">
                <button class="btn btn-primary float-right ml-auto" v-b-modal.add-modal>Add Popup Survey</button>
            </div>
        </div>
        <div class="table-responsive">
            <table class="table">
                <tr>
                    <th>Question<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'question'; searchData.sortOrder='asc';getPopupSurveyList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'question'; searchData.sortOrder='desc';getPopupSurveyList()"></i></th>
                    <th>Option 1<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'option_1'; searchData.sortOrder='asc';getPopupSurveyList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'option_1'; searchData.sortOrder='desc';getPopupSurveyList()"></i></th>
                    <th>Option 2<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'option_2'; searchData.sortOrder='asc';getPopupSurveyList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'option_2'; searchData.sortOrder='desc';getPopupSurveyList()"></i></th>
                    <th>Option 3<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'option_3'; searchData.sortOrder='asc';getPopupSurveyList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'option_3'; searchData.sortOrder='desc';getPopupSurveyList()"></i></th>
                    <th>Option 4<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'option_4'; searchData.sortOrder='asc';getPopupSurveyList()"></i>
            <i class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'option_4'; searchData.sortOrder='desc';getPopupSurveyList()"></i></th>
                    <th>Action</th>
                </tr>
                <tr v-if="popupSurveyLength" v-for="p in popupSurvey.data" v-bind:key="p.id">
                    <td>{{ p.question }}</td>
                    <td>{{ p.option_1 }}</td>
                    <td>{{ p.option_2 }}</td>
                    <td>{{ p.option_3 }}</td>
                    <td>{{ p.option_4 }}</td>
                    <td>
                        <div class="d-flex align-items-center p-0" style="min-width: 100px;">
                            <a type="button" class="mx-3 d-block" width="24" @click="getPopupSurvey(p)">
                                <img src="../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
                            </a>
                            <a type="button" class="mx-3 d-block" width="24" @click="deletePopupSurvey(p.id)">
                                <img src="../../assets/images/table-delete.svg" alt="table-delete" width="24"
                                    height="24" />
                            </a>
                            <router-link type="button" class="mx-3 d-block" width="24"
                                :to="'/admin/popup-survey/' + p.id">
                                <img src="../../assets/images/table-eye.svg" alt="table-eye" width="24" height="24" />
                            </router-link>
                        </div>
                    </td>
                </tr>
                <tr v-if="!popupSurveyLength">
                    <td colspan="5">No Data Found</td>
                </tr>
            </table>
        </div>
        <b-modal id="add-modal" size="lg" title="Add New Popup Survey" :hide-footer=hideFooter no-fade no-enforce-focus>
            <form>
                <div class="form-group">
                    <label>Question <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addData.question" placeholder="Question" />
                </div>
                <div class="form-group">
                    <label>Option 1 <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addData.option_1" placeholder="Option 1" />
                </div>
                <div class="form-group">
                    <label>Option 2 <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addData.option_2" placeholder="Option 2" />
                </div>
                <div class="form-group">
                    <label>Option 3 <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addData.option_3" placeholder="Option 3" />
                </div>
                <div class="form-group">
                    <label>Option 4 <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="addData.option_4" placeholder="Option 4" />
                </div>
                <div class="form-group">
                    <button type="button" @click="addPopupSurvey" class="btn btn-primary"
                        :disabled="addData.disabled">Submit</button>
                </div>
            </form>
        </b-modal>
        <b-modal id="update-modal" size="lg" title="Update Popup Survey" :hide-footer=hideFooter no-fade
            no-enforce-focus>
            <form>
                <div class="form-group">
                    <label>Question <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateData.question" placeholder="Question" />
                </div>
                <div class="form-group">
                    <label>Option 1 <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateData.option_1" placeholder="Option 1" />
                </div>
                <div class="form-group">
                    <label>Option 2 <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateData.option_2" placeholder="Option 2" />
                </div>
                <div class="form-group">
                    <label>Option 3 <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateData.option_3" placeholder="Option 3" />
                </div>
                <div class="form-group">
                    <label>Option 4 <span class="err">*</span></label>
                    <input class="form-control" type="text" v-model="updateData.option_4" placeholder="Option 4" />
                </div>
                <div class="form-group">
                    <button type="button" @click="updatePopupSurvey" class="btn btn-primary"
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
    name: 'PopupSurveys',
    mixins: [AppMixin],
    data() {
        return {
            hideFooter: true,
            addData: {
                question: '',
                option_1: '',
                option_2: '',
                option_3: '',
                option_4: '',
                disabled: false
            },
            updateData: {
                id: '',
                question: '',
                option_1: '',
                option_2: '',
                option_3: '',
                option_4: '',
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

        addPopupSurvey: function (e) {
            let that = this;
            if (!that.addData.question || !that.addData.option_1 || !that.addData.option_2 || !that.addData.option_3 || !that.addData.option_4) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required  fields",
                    showConfirmButton: true
                })
            } else {
                that.addData.disabled = true;
                Api.addPopupSurvey(that.addData).then(response => {
                    that.addData.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Popup Survey added successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.addData.disabled = false
                        that.addData.question = ''
                        that.addData.option_1 = ''
                        that.addData.option_2 = ''
                        that.addData.option_3 = ''
                        that.addData.option_4 = ''
                        that.$bvModal.hide('add-modal')
                        that.getPopupSurveyList()
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
        getPopupSurvey: function (data) {
            let that = this;
            that.updateData.id = data.id
            that.updateData.question = data.question
            that.updateData.option_1 = data.option_1
            that.updateData.option_2 = data.option_2
            that.updateData.option_3 = data.option_3
            that.updateData.option_4 = data.option_4
            that.$bvModal.show('update-modal')
        },
        updatePopupSurvey: function (e) {
            let that = this;
            if (!that.updateData.question || !that.updateData.option_1 || !that.updateData.option_2 || !that.updateData.option_3 || !that.updateData.option_4) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required  fields",
                    showConfirmButton: true
                })
            } else {
                that.updateData.disabled = true;
                Api.updatePopupSurvey(that.updateData).then(response => {
                    that.updateData.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Popup Survey updated successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.updateData.disabled = false
                        that.updateData.question = ''
                        that.updateData.option_1 = ''
                        that.updateData.option_2 = ''
                        that.updateData.option_3 = ''
                        that.updateData.option_4 = ''
                        that.$bvModal.hide('update-modal')
                        that.getPopupSurveyList()
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
        deletePopupSurvey: function (id) {
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
                    Api.deletePopupSurvey(id).then(response => {
                        this.$swal({
                            icon: "success",
                            title: "Success",
                            text: "Popup Survey deleted successfully",
                            showConfirmButton: true
                        }).then(function () {
                            that.getPopupSurveyList()
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
        getPopupSurveyList: function () {
            let that = this
            Api.getPopupSurveyList(that.searchData).then(response => {
                that.popupSurvey = response.data.res
                that.popupSurveyLength = that.popupSurvey.data.length
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
        this.getPopupSurveyList()
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
