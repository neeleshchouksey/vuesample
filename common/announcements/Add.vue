<template>
    <!--Add announcement modal popup-->
    <b-modal id="add-announcement-modal" title="Add Announcement" :hide-footer=hideFooter>
        <form>
            <div class="form-group">
                <label>Title<span class="err">*</span></label>
                <input type="text" class="form-control" v-model="announcements.title" :maxlength=totalTitleChar
                    id="title" placeholder="Title" @keypress="titleCharCount" />
                <p>Remaining Characters {{ remainingTitleChar }}</p>
            </div>
            <div class="form-group">
                <label>Description<span class="err">*</span></label>
                <textarea type="text" class="form-control" id="description" placeholder="Description"
                    @keypress="descriptionCharCount" v-model="announcements.description"
                    :maxlength=totalDescriptionChar></textarea>
                <p>Remaining Characters {{ remainingDescriptionChar }}</p>
            </div>
            <div class="form-group">
                <label>Select Date<span class="err">*</span></label><br>
                <date-picker v-model="announcements.date" type="datetime" :use12h="format" placeholder="Date"
                    :disabled-date="disabledRange"></date-picker>
            </div>
            <button type="button" @click="addAnnouncement" class="btn btn-primary"
                :disabled="announcements.disabled">Submit</button>
        </form>
    </b-modal>
</template>
<script>
/* eslint-disable */
import Api from '../../../router/api'
import AppMixin from '../../../mixins/AppMixin'
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
import moment from 'moment'

export default {
    name: 'Add',
    props: [
        'getAnnouncementsList'
    ],
    mixins: [AppMixin],
    data() {
        return {
            hideFooter: true,
            format: true,
            announcements: {
                'title': '',
                'description': '',
                'date': '',
                'disabled': false
            },
            totalDescriptionChar: 750,
            totalTitleChar: 50,
            remainingDescriptionChar: 750,
            remainingTitleChar: 50,
            disabledDates: {
                to: new Date(Date.now() - 8640000)
            }
        }
    },
    components: { DatePicker },
    methods: {

        addAnnouncement: function () {
            let that = this;
            if (!that.announcements.title || !that.announcements.description || !that.announcements.date) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required fields",
                    showConfirmButton: true
                });
            } else {
                that.announcements.disabled = true;
                Api.addAnnouncement(that.announcements).then(response => {
                    that.announcements.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Anouncement added successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.announcements.disabled = false
                        that.announcements.title = ''
                        that.announcements.description = ''
                        that.announcements.date = ''
                        that.$bvModal.hide('add-announcement-modal')
                        that.getAnnouncementsList();
                    });
                }
                ).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.announcements.disabled = false;
                    });
                });
            }
        },
        descriptionCharCount: function () {
            this.remainingDescriptionChar = this.totalDescriptionChar - this.announcements.description.length
        },
        titleCharCount: function () {
            this.remainingTitleChar = this.totalTitleChar - this.announcements.title.length
        },
    },
    mounted() {
    }
}
</script>