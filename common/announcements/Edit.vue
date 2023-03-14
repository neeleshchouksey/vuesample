<template>
    <b-modal id="update-announcement-modal" title="Update Announcement" :hide-footer=hideFooter>
        <form>
            <div class="form-group">
                <label>Title <span class="err">*</span></label>
                <input type="text" class="form-control" v-model="announcementUpdate.title" :maxlength=totalTitleChar
                    id="title" placeholder="Title" @keypress="titleCharCountUpdate" />
                <p>Remaining Characters {{ remainingTitleChar }}</p>
            </div>
            <div class="form-group">
                <label>Description <span class="err">*</span></label>
                <textarea type="text" class="form-control" id="description" placeholder="Description"
                    @keypress="descriptionCharCountUpdate" v-model="announcementUpdate.description"
                    :maxlength=totalDescriptionChar></textarea>
                <p>Remaining Characters {{ remainingDescriptionChar }}</p>
            </div>
            <div class="form-group">
                <label>Select Date <span class="err">*</span></label><br>
                <date-picker v-model="announcementUpdate.date" type="datetime" :use12h="format" placeholder="Date"
                    :default-value="announcementUpdate.date" :disabled-date="disabledRange"></date-picker>
            </div>
            <button type="button" @click="updateAnnouncement" class="btn btn-primary"
                :disabled="announcementUpdate.disabled">Submit</button>
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
    name: 'Edit',
    mixins: [AppMixin],
    props: [
        'getAnnouncementsList',
        'announcementUpdate',
    ],
    data() {
        return {
            format: true,
            hideFooter: true,
            disabledDates: {
                to: new Date(Date.now() - 8640000)
            },
            totalDescriptionChar: 750,
            totalTitleChar: 50,
            remainingDescriptionChar: 750,
            remainingTitleChar: 50,
        }
    },
    components: { DatePicker },
    methods: {
        updateAnnouncement: function () {
            let that = this;
            if (!that.announcementUpdate.title || !that.announcementUpdate.description || !that.announcementUpdate.date) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required fields",
                    showConfirmButton: true
                });
            } else {
                that.announcementUpdate.disabled = true;

                Api.updateAnnouncement(that.announcementUpdate).then(response => {
                    that.announcementUpdate.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Announcement details updated successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.announcementUpdate.disabled = false;
                        that.$bvModal.hide('update-announcement-modal')
                        that.getAnnouncementsList()
                    });
                }
                ).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.announcementUpdate.disabled = false;
                    });
                });
            }
        },
        descriptionCharCountUpdate: function () {
            this.remainingDescriptionChar = this.totalDescriptionChar - this.announcementUpdate.description.length
        },
        titleCharCountUpdate: function () {
            this.remainingTitleChar = this.totalTitleChar - this.announcementUpdate.title.length
        },

    },
    mounted() {
    }
}
</script>
