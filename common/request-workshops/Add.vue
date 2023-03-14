<template>
    <b-modal id="request-workshop-modal" size="lg" title="Request Workshop" :hide-footer=hideFooter no-fade no-enforce-focus >
        <form>
            <!-- <div class="form-group">
                <label>Select Company <span class="err">*</span></label>
                <select class="form-control" v-model="workshop.company">
                    <option selected value="">Select</option>
                    <option v-for="cl in companiesList" :value="cl.id" v-bind:key="cl.id">{{ cl.company_name
                    }}
                    </option>
                </select>
            </div> -->
            <div class="form-group">
                <label>Title <span class="err">*</span></label>
                <input class="form-control" type="text" v-model="workshop.name" placeholder="Name" />
            </div>
            <div class="form-group">
                <label>Workshop Focus <span class="err">*</span></label>
                <input type="text" class="form-control" v-model="workshop.workshop_focus"
                    placeholder="Workshop Focus" />
            </div>
            <div class="form-group">
                <label>Desired Date <span class="err">*</span></label>
                <date-picker v-model="workshop.desired_date" type="datetime" :use12h="format" placeholder="Desired Date"
                    :disabled-date="disabledRange"></date-picker>
            </div>
            <div class="form-group">
                <label>Workshop Length <span class="err">*</span></label>
                <select class="form-control" v-model="workshop.workshop_length">
                    <option value="">Select</option>
                    <option>1 hr</option>
                    <option>2 hrs</option>
                </select>
            </div>
            <div class="form-group">
                <label>Workshop Type <span class="err">*</span></label>
                <select class="form-control" v-model="workshop.workshop_type">
                    <option value="">Select</option>
                    <option value="IN_PERSON">In Person</option>
                    <option value="VIRTUAL">Virtual</option>
                </select>
            </div>
            <div class="form-group">
                <label>Audience</label>
                <textarea class="form-control" placeholder="Audience" v-model="workshop.audience"></textarea>
            </div>
            <div class="form-group">
                <label>Requirements</label>
                <textarea class="form-control" placeholder="Requirements" v-model="workshop.requirements"></textarea>
            </div>
            <div class="form-group">
                <label>Expectations</label>
                <textarea class="form-control" placeholder="Expectations" v-model="workshop.expectations"></textarea>
            </div>
            <div class="form-group">
                <button type="button" class="btn btn-primary" @click="requestWorkshop"
                    :disabled="workshop.disabled">Submit</button>
            </div>
        </form>
    </b-modal>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';
export default {
    name: 'Add',
    mixins: [AppMixin],
    props: {
        getWorkshopList: {
            type: Function
        }
    },
    data() {
        return {
            format: true,
            hideFooter: true,
            workshop: {
                // company: '',
                name: '',
                workshop_focus: '',
                desired_date: '',
                workshop_length: '',
                workshop_type: '',
                audience: '',
                requirements: '',
                expectations: '',
                disabled: false
            },
        }
    },
    components: { DatePicker },
    methods: {
        requestWorkshop: function () {
            let that = this;
            console.log(that.workshop)
            if (!that.workshop.name || !that.workshop.workshop_focus || !that.workshop.desired_date ||
                !that.workshop.workshop_length || !that.workshop.workshop_type) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required fields",
                    showConfirmButton: true
                })
            } else {
                that.workshop.disabled = true;
                Api.requestWorkshop(that.workshop).then(response => {
                    that.workshop.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Workshop requested successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.workshop.disabled = false
                        that.workshop.name = ''
                        that.workshop.company = ''
                        that.workshop.workshop_focus = ''
                        that.workshop.workshop_length = ''
                        that.workshop.workshop_type = ''
                        that.workshop.workshop_focus = ''
                        that.workshop.desired_date = ''
                        that.workshop.audience = ''
                        that.workshop.requirements = ''
                        that.workshop.expectations=''
                        that.$bvModal.hide('request-workshop-modal')
                        that.getWorkshopList()
                    });
                }).catch((error) => {
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
        this.getCompaniesList()
    }
}
</script>
