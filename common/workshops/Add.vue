<template>
    <b-modal id="add-workshop-modal" title="Add New Workshop" :hide-footer=hideFooter size="lg" no-fade
        no-enforce-focus>
        <form enctype="multipart/form-data" @submit="addWorkshop">
            <div id="details">
                <div class="form-group">
                    <label>Select Company <span class="err">*</span></label>
                    <multiselect v-model="workshop.company" :options="companiesListMultiselect" group-values="values"
                        group-label="selectAll" :multiple="true" :group-select="true" placeholder="Type to search"
                        track-by="name" label="name">
                        <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
                    </multiselect>
                </div>
                <div class="form-group">
                    <label>Title <span class="err">*</span></label>
                    <input type="text" class="form-control" id="title" placeholder="Title" v-model="workshop.title">
                </div>
                <div class="form-group">
                    <label>Description<span class="err">*</span></label>
                    <textarea class="form-control" id="description" placeholder="Description"
                        v-model="workshop.description"></textarea>
                </div>
                <div class="form-group">
                    <label>Total Hours <span class="err">*</span></label>
                    <input type="text" class="form-control" id="total_hours" placeholder="Total Hours"
                        v-model="workshop.total_hours">
                </div>
                <div class="form-group">
                    <label>Date <span class="err">*</span></label>
                      <date-picker v-model="workshop.date" type="datetime" :use12h="format" placeholder="Date"
                    :disabled-date="disabledRange"></date-picker>
                </div>
                <div class="form-group">
                    <label>Instructor <span class="err">*</span></label>
                    <input type="text" class="form-control" id="instructor" placeholder="Instructor"
                        v-model="workshop.instructor">
                </div>
                <div class="form-group">
                    <label>Meeting Type <span class="err">*</span></label>
                    <select class="form-control" v-model="workshop.meeting_type">
                        <option value="IN_PERSON">In Person</option>
                        <option value="ZOOM">Zoom</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Additional Info<span class="err">*</span></label>
                    <vue2-tinymce-editor v-model="workshop.additional_info" placeholder="Additional Info">
                    </vue2-tinymce-editor>
                </div>
                <div class="form-group">
                    <label>Upload Image <span class="err">*</span></label>
                    <input type="file" class="form-control" id="image" ref="image" @change="fileOnChange">
                </div>
                <button type="submit" class="btn btn-primary" :disabled="workshop.disabled">Submit</button>
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
    name: 'Add',
    mixins: [AppMixin],
    components: {
        Vue2TinymceEditor,DatePicker
    },
    props: {
        getWorkshopsLists: {
            type: Function
        }
    },
    data() {
        return {
            format: true,
            hideFooter: true,
            workshop: {
                'company': '',
                'title': '',
                'description': '',
                'image': '',
                'total_hours': '',
                'date': '',
                'instructor': '',
                'additional_info': '',
                'meeting_type': '',
                'disabled': false,
            },
        }
    },
    methods: {
        fileOnChange: function (e) {
            this.workshop.image = this.$refs.image.files[0];
        },
        addWorkshop: function (e) {
            e.preventDefault();
            let that = this;
            if (!that.workshop.company || !that.workshop.title || !that.workshop.description || !that.workshop.image || !that.workshop.total_hours || !that.workshop.instructor || !that.workshop.date || !that.workshop.additional_info) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required fields fields",
                    showConfirmButton: true
                });
            } else {
                let that = this
                that.workshop.disabled = true;
                const formData = new FormData();
                formData.append('image', that.workshop.image);
                formData.append('title', that.workshop.title)
                formData.append('description', that.workshop.description)
                formData.append('total_hours', that.workshop.total_hours)
                formData.append('instructor', that.workshop.instructor)
                formData.append('date', moment(String(that.workshop.date)).format('MM/DD/YYYY HH:mm:ss'))
                formData.append('additional_info', that.workshop.additional_info)
                formData.append('meeting_type', that.workshop.meeting_type)
                formData.append('company', JSON.stringify(that.workshop.company))
                let headers = {
                    'Content-Type': 'multipart/form-data',
                    'Access-Control-Allow-Origin': '*'
                }
                Api.addWorkshop(formData, headers).then(response => {
                    that.workshop.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Workshop created successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.workshop.title = ''
                        that.workshop.description = ''
                        that.workshop.total_hours = ''
                        that.workshop.instructor = ''
                        that.workshop.date = ''
                        that.workshop.meeting_type = ''
                        that.workshop.company = ''
                        that.workshop.additional_info = ''
                        that.$refs.image.value = null;
                        that.$bvModal.hide('add-workshop-modal')
                        that.getWorkshopsLists()
                    });
                }
                ).catch((error) => {
                    that.workshop.disabled = false;
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.workshop.disabled = false;
                    });
                });
            }
        },
    },
    mounted() {
        this.getCompaniesListMultiselect()
    }
}
</script>
