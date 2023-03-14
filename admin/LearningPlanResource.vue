<template>
    <section class="popup-survey-view-section half-cut-bg">
        <h1 class="page-title text-left">Learning Plan Resources</h1>
        <div class="row">
            <div class="col-md-3 my-2 d-flex align-items-center">
                <input type="text" v-model="searchData.keyword" class="form-control mb-0 search" placeholder="Search"
                    v-on:keyup="getLearningPlanResources" /><span class="search-icon"></span><a href="javascript:void(0)"
                    v-on:click="searchData.keyword = ''; getLearningPlanResources()" class="link px-2 ">clear</a>
            </div>
            <div class="col-md-3 my-2">
            </div>
            <div class="col-md-6 d-flex my-2" v-if="user.role == 'ADMIN'">
                <button class="btn btn-primary ml-auto" v-b-modal.add-modal>Add Learning Plan File</button>
            </div>
        </div>
        <div class="table-responsive">
            <table class="table">
                <tr>
                    <th>Title<i class="fa-solid fa-arrow-up"
                            @click="searchData.sortBy = 'title'; searchData.sortOrder = 'asc'; getLearningPlanResources()"></i>
                        <i class="fa-solid fa-arrow-down"
                            @click="searchData.sortBy = 'title'; searchData.sortOrder = 'desc'; getLearningPlanResources()"></i>
                    </th>
                    <th>Description<i class="fa-solid fa-arrow-up"
                            @click="searchData.sortBy = 'title'; searchData.sortOrder = 'asc'; getLearningPlanResources()"></i>
                        <i class="fa-solid fa-arrow-down"
                            @click="searchData.sortBy = 'title'; searchData.sortOrder = 'desc'; getLearningPlanResources()"></i>
                    </th>
                    <th>Link</th>
                    <th>File</th>
                    <th>Action</th>
                </tr>
                <tr v-if="planFiles.length" v-for="lp in planFiles" v-bind:key="lp.id">
                    <td>{{ lp.title }}</td>
                    <td>{{ lp.description }}</td>
                    <td><a v-if="lp.link" :href="lp.link" target="_blank">Watch Video</a> <span v-else>NA</span></td>
                    <td><a v-if="lp.image" class="cursor-pointer links" @click="downloadLearningPlanFile(lp.id, lp.image)">Download</a> <span v-else>NA</span>
                    </td>
                    <td>
                        <div class="d-flex align-items-center p-0" style="min-width: 100px;">

                            <a type="button" class="mx-3 d-block" width="24" @click="getLearningPlanFile(lp)">
                                <img src="../../assets/images/table-edit.svg" alt="table-edit" width="24" height="24" />
                            </a>
                            <a type="button" class="mx-3 d-block" width="24" @click="deleteLearningPlanFile(lp.id)">
                                <img src="../../assets/images/table-delete.svg" alt="table-delete" width="24"
                                    height="24" />
                            </a>
                        </div>
                        <!-- <button class="btn btn-primary" @click="getLearningPlanFile(lp)"><i
                      class="fa fa-pencil"></i></button>
              <button class="btn btn-danger" @click="deleteLearningPlanFile(lp.id)"><i
                      class="fa fa-trash"></i></button> -->
                    </td>
                </tr>
                <tr v-if="!planFiles.length">
                    <td colspan="5">No Data Found</td>
                </tr>
            </table>
        </div>
        <b-modal id="add-modal" title="Add New Learning Plan File" :hide-footer=hideFooter>
            <form enctype="multipart/form-data">
                <div id="details">
                    <div class="form-group">
                        <label>Title <span class="err">*</span></label>
                        <input type="text" class="form-control" id="title" placeholder="Title" :maxlength=totalTitleChar
                            @keypress="titleCharCount" v-model="plan.title">
                        <p>Remaining Characters {{ remainingTitleChar }}</p>

                    </div>
                    <div class="form-group">
                        <label>Description<span class="err">*</span></label>
                        <textarea class="form-control" id="description" placeholder="Description"
                            :maxlength=totalDescriptionChar @keypress="descriptionCharCount"
                            v-model="plan.description"></textarea>
                        <p>Remaining Characters {{ remainingDescriptionChar }}</p>
                    </div>
                    <div class="form-group">
                        <label>Video Link <span class="err">*</span></label>
                        <input type="text" class="form-control" id="title" placeholder="Video Link"
                         v-model="plan.link">
                    </div>
                    <div class="form-group">
                        <label>File<span class="err">*</span></label>
                        <input type="file" class="form-control" id="image" ref="image" @change="imageOnChange">
                    </div>
                    <button type="button" class="btn btn-primary" @click="addLearningPlanFile"
                        :disabled="plan.disabled">Submit
                    </button>
                </div>
            </form>
        </b-modal>
        <b-modal id="update-modal" title="Update Learning Plan File" :hide-footer=hideFooter>
            <form enctype="multipart/form-data">
                <div id="details">
                    <div class="form-group">
                        <label>Title <span class="err">*</span></label>
                        <input type="text" class="form-control" id="title" placeholder="Title" :maxlength=totalTitleChar
                            @keypress="titleCharCountUpdate" v-model="planUpdate.title">
                        <p>Remaining Characters {{ remainingTitleChar }}</p>
                    </div>
                    <div class="form-group">
                        <label>Description<span class="err">*</span></label>
                        <textarea class="form-control" id="description" placeholder="Description"
                            :maxlength=totalDescriptionChar v-model="planUpdate.description"
                            @keypress="descriptionCharCountUpdate"></textarea>
                        <p>Remaining Characters {{ remainingDescriptionChar }}</p>
                    </div>
                    <div class="form-group">
                        <label>Video Link <span class="err">*</span></label>
                        <input type="text" class="form-control" id="title" placeholder="Video Link"
                         v-model="planUpdate.link">
                    </div>
                    <div class="form-group">
                        <label>File<span class="err">*</span></label>
                        <input type="file" class="form-control" id="image" ref="imageUpdate"
                            @change="imageOnChangeUpdate">
                    </div>
                    <button type="button" class="btn btn-primary" @click="updateLearningPlanFile"
                        :disabled="planUpdate.disabled">Submit
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
    name: 'LearningPlan',
    mixins: [AppMixin],
    data() {
        return {
            filePath: '',
            hideFooter: true,
            totalDescriptionChar: 300,
            totalTitleChar: 30,
            remainingDescriptionChar: 300,
            remainingTitleChar: 30,
            plan: {
                'title': '',
                'description': '',
                'link': '',
                'image': '',
                'disabled': false
            },

            planUpdate: {
                'id': '',
                'profile_type': '',
                'title': '',
                'description': '',
                'link': '',
                'image': '',
                'disabled': false
            },
            searchData: {
                'sortBy': '',
                'sortOrder': '',
                'keyword': ''
            }
        }
    },
    components: {},
    methods: {

        deleteLearningPlanFile: function (id) {
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
                    Api.deleteLearningPlanFile(id).then(response => {
                        this.$swal({
                            icon: 'success',
                            title: 'Success',
                            text: 'Todo deleted successfully',
                            showConfirmButton: true
                        }).then(function () {
                            that.getLearningPlanResources()
                        })
                    }).catch((error) => {
                        this.$swal({
                            icon: 'error',
                            title: 'error',
                            text: error.response.data.message,
                            showConfirmButton: true
                        }).then(function () {
                            that.todo.disabled = false
                        })
                    })
                }
            })
        },
        getLearningPlanFile: function (data) {
            this.planUpdate.id = data.id
            this.planUpdate.title = data.title
            this.planUpdate.description = data.description
            this.$bvModal.show('update-modal')
        },
        imageOnChange: function (e) {
            let that = this
            this.plan.image = this.$refs.image.files[0]
        },
        imageOnChangeUpdate: function (e) {
            let that = this
            this.planUpdate.image = this.$refs.imageUpdate.files[0]
        },
        addLearningPlanFile: function (e) {
            e.preventDefault()
            let that = this
            if (!that.plan.title || !that.plan.description || !that.plan.image) {
                this.$swal({
                    icon: 'error',
                    title: 'error',
                    text: 'Please all required fields',
                    showConfirmButton: true
                })
            }else if (that.plan.link == '' && that.plan.image == '') {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill link or upload file fields",
                    showConfirmButton: true
                });
            } 
             else {
                that.plan.disabled = true
                const formData = new FormData()
                formData.append('image', that.plan.image)
                formData.append('description', that.plan.description)
                formData.append('title', that.plan.title)
                formData.append('link', that.plan.link)
                Api.addLearningPlanFile(formData).then(response => {
                    that.plan.disabled = false

                    let headers = {
                        'Content-Type': 'multipart/form-data',
                        'Access-Control-Allow-Origin': '*'
                    }
                    this.$swal({
                        icon: 'success',
                        title: 'Success',
                        text: 'Learning Plan File created successfully',
                        showConfirmButton: true
                    }).then(function () {
                        that.$bvModal.hide('add-modal')
                        that.plan.title = ''
                        that.plan.description = ''
                        that.$refs.image.value = null
                        that.getLearningPlanResources()
                    })
                }
                ).catch((error) => {
                    that.plan.disabled = false
                    this.$swal({
                        icon: 'error',
                        title: 'error',
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.plan.disabled = false
                    })
                })
            }
        },
        updateLearningPlanFile: function (e) {
            e.preventDefault()
            let that = this
            if (!that.planUpdate.title || !that.planUpdate.description) {
                this.$swal({
                    icon: 'error',
                    title: 'error',
                    text: 'Please all required fields',
                    showConfirmButton: true
                })
            } else {
                that.plan.disabled = true
                const formData = new FormData()
                formData.append('image', that.planUpdate.image)
                formData.append('description', that.planUpdate.description)
                formData.append('title', that.planUpdate.title)
                formData.append('id', that.planUpdate.id)
                formData.append('link', that.planUpdate.link)

                Api.updateLearningPlanFile(formData).then(response => {
                    that.planUpdate.disabled = false

                    let headers = {
                        'Content-Type': 'multipart/form-data',
                        'Access-Control-Allow-Origin': '*'
                    }
                    this.$swal({
                        icon: 'success',
                        title: 'Success',
                        text: 'Learning Plan File updated successfully',
                        showConfirmButton: true
                    }).then(function () {
                        that.$bvModal.hide('update-modal')
                        that.planUpdate.title = ''
                        that.planUpdate.description = ''
                        that.$refs.imageUpdate.value = null
                        that.getLearningPlanResources()
                    })
                }
                ).catch((error) => {
                    that.plan.disabled = false
                    this.$swal({
                        icon: 'error',
                        title: 'error',
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.plan.disabled = false
                    })
                })
            }
        },
        getLearningPlanResources: function () {
            let that = this
            Api.getLearningPlanResources(that.searchData).then(response => {
                that.planFiles = response.data.res
                that.filePath = response.data.path
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
        descriptionCharCount: function () {
            this.remainingDescriptionChar = this.totalDescriptionChar - this.plan.description.length
        },
        titleCharCount: function () {
            this.remainingTitleChar = this.totalTitleChar - this.plan.title.length
        },
        descriptionCharCountUpdate: function () {
            this.remainingDescriptionChar = this.totalDescriptionChar - this.planUpdate.description.length
        },
        titleCharCountUpdate: function () {
            this.remainingTitleChar = this.totalTitleChar - this.planUpdate.title.length
        },
    },
    mounted() {
        this.getProfileTypeList()
        this.getLearningPlanResources()
    }
}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  
  </style>
  