<template>
    <b-modal id="update-workshop-modal" title="Update Workshop" :hide-footer=hideFooter size="lg" no-fade
        no-enforce-focus>
        <form enctype="multipart/form-data" @submit="updateWorkshop">
            <div id="details">
                <div class="form-group">
                    <label>Select Company <span class="err">*</span></label>
                    <multiselect v-model="workshopUpdateData.companies" :options="companiesListMultiselect"
                        group-values="values" group-label="selectAll" :multiple="true" :group-select="true"
                        placeholder="Type to search" track-by="name" label="name">
                        <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
                    </multiselect>
                </div>
                <div class="form-group">
                    <label>Title <span class="err">*</span></label>
                    <input type="text" class="form-control" id="title" placeholder="Title"
                        v-model="workshopUpdateData.title">
                </div>
                <div class="form-group">
                    <label>Description<span class="err">*</span></label>
                    <textarea class="form-control" id="description" placeholder="Description"
                        v-model="workshopUpdateData.description"></textarea>
                </div>
                <div class="form-group">
                    <label>Total Hours <span class="err">*</span></label>
                    <input type="text" class="form-control" id="total_hours" placeholder="Total Hours"
                        v-model="workshopUpdateData.total_hours">
                </div>
                <div class="form-group">
                    <label>Date <span class="err">*</span></label>
                      <date-picker v-model="workshopUpdateData.date" type="datetime" :use12h="format" placeholder="Date"
                   :default-value="workshopUpdateData.date"  :disabled-date="disabledRange"></date-picker>
                </div>
                <div class="form-group">
                    <label>Instructor <span class="err">*</span></label>
                    <input type="text" class="form-control" id="instructor" placeholder="Instructor"
                        v-model="workshopUpdateData.instructor">
                </div>
                <div class="form-group">
                    <label>Meeting Type <span class="err">*</span></label>
                    <select class="form-control" v-model="workshopUpdateData.meeting_type">
                        <option value="IN_PERSON">In Person</option>
                        <option value="ZOOM">Zoom</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Additional Info<span class="err">*</span></label>
                    <vue2-tinymce-editor v-model="workshopUpdateData.additional_info" placeholder="Additional Info">
                    </vue2-tinymce-editor>
                </div>
                <div class="form-group">
                    <label>Upload Image <span class="err">*</span></label>
                    <input type="file" class="form-control" id="imageUpdate" ref="imageUpdate"
                        @change="fileOnChangeUpdate">
                </div>
                <button type="submit" class="btn btn-primary" :disabled="workshopUpdateData.disabled">Submit</button>
            </div>
        </form>
    </b-modal>
</template>
<style src="vue-multiselect/dist/vue-multiselect.min.css">
</style>
<script>
/* eslint-disable */
import moment from 'moment'
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import DatePicker from 'vue2-datepicker';
import { Vue2TinymceEditor } from "vue2-tinymce-editor";
import 'vue2-datepicker/index.css';
export default {
    name: 'Edit',
    mixins: [AppMixin],
    props: [
        'getWorkshopsLists',
        'workshopUpdateData'
    ],
    components: {
        Vue2TinymceEditor, DatePicker
    },
    data() {
        return {
            format: true,
            hideFooter: true,
        }
    },
    methods: {
        fileOnChangeUpdate: function () {
            this.workshopUpdateData.image = this.$refs.imageUpdate.files[0];
        },
        updateWorkshop: function (e) {
            e.preventDefault()
            let that = this;
            console.log(that.workshopUpdateData)
            if (!that.workshopUpdateData.title || !that.workshopUpdateData.description || !that.workshopUpdateData.total_hours || !that.workshopUpdateData.instructor || !that.workshopUpdateData.date || !that.workshopUpdateData.additional_info) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required fields",
                    showConfirmButton: true
                });
            } else {
                that.workshopUpdateData.disabled = true;
                const formData = new FormData();
                formData.append('id', that.workshopUpdateData.id);
                formData.append('image', that.workshopUpdateData.image)
                formData.append('title', that.workshopUpdateData.title)
                formData.append('description', that.workshopUpdateData.description)
                formData.append('total_hours', that.workshopUpdateData.total_hours)
                formData.append('instructor', that.workshopUpdateData.instructor)
                formData.append('date', moment(String(that.workshopUpdateData.date)).format('MM/DD/YYYY HH:mm:ss'))
                formData.append('additional_info', that.workshopUpdateData.additional_info)
                formData.append('meeting_type', that.workshopUpdateData.meeting_type)
                formData.append('company', JSON.stringify(that.workshopUpdateData.companies))
                let headers = {
                    'Content-Type': 'multipart/form-data',
                    'Access-Control-Allow-Origin': '*'
                }
                Api.updateWorkshop(formData, headers).then(response => {
                    that.workshopUpdateData.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Workshop details updated successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.workshopUpdateData.disabled = false;
                        that.$bvModal.hide('update-workshop-modal')
                        that.getWorkshopsLists()
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.workshopUpdateData.disabled = false;
                    });
                });
            }
        },
    },
    mounted() {
        this.getCompaniesListMultiselect()
    },
}
</script>
