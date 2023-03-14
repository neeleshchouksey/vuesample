<template>
    <form class="login-from" @submit="validateForm" ref="createCompanyForm" enctype="multipart/form-data">
        <div class="row">
            <div class="col-md-12" v-if="modalAdd">
                <div class="form-group">
                    <!-- <select v-modal="chargebeeUser.planData" class="form-control">
                        <option value="">Select Plan</option>
                        <option v-for="p in plans" v-bind:key="p.id" v-bind:value="p">
                            {{ p.name }}
                        </option>
                    </select> -->

                    <multiselect v-model="chargebeeUser.planData" :options="plans" placeholder="Type to search"
                        track-by="name" label="name">
                        <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
                    </multiselect>
                </div>
            </div>
            <div class="col-md-12">
                <div class="form-group">
                    <input type="text" class="form-control" id="companyname" placeholder="Company/Organization Name *"
                        v-model="chargebeeUser.companyname">
                </div>
            </div>
            <div class="col-md-12">
                <div class="form-group">
                    <input type="email" class="form-control" id="email"
                        placeholder="Company/Organization Representative Email *" v-model="chargebeeUser.email">
                </div>
            </div>
            <div class="col-md-12">
                <div class="form-group">
                    <input type="password" class="form-control" id="password" placeholder="Password *"
                        v-model="chargebeeUser.password">
                </div>
            </div>
             <div class="col-md-12">
                <div class="form-group">
                    <input type="password" class="form-control" id="cpassword" placeholder="Confirm Password *"
                        v-model="chargebeeUser.cpassword">
                </div>
            </div>
            <div class="col-md-12">
                <div class="form-group">
                    <input type="text" class="form-control" id="companydomain" placeholder="Company Url *"
                        v-model="chargebeeUser.domain">
                </div>
            </div>
            <div class="col-md-12">
                <div class="form-group">
                    <input type="number" class="form-control" id="employees" placeholder="Number of Employees *"
                        v-model="chargebeeUser.employees">
                </div>
            </div>
            <!-- <div class="col-md-12">
                <div class="form-group">
                    <div class="custom-file">
                        <input type="file" class="custom-file-input" id="logo" ref="logo" @change="fileOnChange">
                        <label class="custom-file-label" for="customFile">Upload Company Logo *</label>
                    </div>
                </div>
                <img v-if="url" :src="url" height="50" width="50"/>
            </div> -->
        </div>
        <button type="submit" class="d-block btn btn-primary" :disabled="chargebeeUser.disabled">Submit</button>
    </form>
</template>
<style src="vue-multiselect/dist/vue-multiselect.min.css">
</style>

<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import axios from 'axios'

export default {
    name: 'Add',
    mixins: [AppMixin],
    props: [
        'modalAdd'
    ],
    data() {
        return {
            chargebeeUser: {
                'companyname': '',
                'email': '',
                'password': '',
                'cpassword':'',
                // 'firstname':'',
                // 'lastname':'',
                'domain': '',
                'employees': '',
                'plan': '',
                'addon': '',
                'logo': '',
                'disabled': false,
                'periodUnit': '',
                'planType': '',
                'planData': ''
                // terms: ''
            },
            subscriptionPlans: [],
            hideFooter: true,
            plans: [],
            url:''
        }
    },
    methods: {
        fileOnChange: function (e) {
            let that = this;
            this.chargebeeUser.logo = this.$refs.logo.files[0];
            this.url = URL.createObjectURL(e.target.files[0]);
        },
        validateForm: function (e) {
            e.preventDefault();
            let that = this;
            if (that.modalAdd != 1) {
                this.chargebeeUser.plan = this.$route.query.plan
                this.chargebeeUser.periodUnit = this.$route.query.periodUnit
                this.chargebeeUser.planType = this.$route.query.itemId
            }
            else {
                this.chargebeeUser.plan = this.chargebeeUser.planData.id
                this.chargebeeUser.periodUnit = this.chargebeeUser.planData.periodUnit
                this.chargebeeUser.planType = this.chargebeeUser.planData.itemId
            }

            if (!that.chargebeeUser.plan) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please select a plan from plans page",
                    showConfirmButton: true
                });
            }
            else if (!that.chargebeeUser.companyname || !that.chargebeeUser.email || !that.chargebeeUser.password ||  !that.chargebeeUser.cpassword || !that.chargebeeUser.domain || !that.chargebeeUser.employees) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required fields",
                    showConfirmButton: true
                });
            }else if(that.chargebeeUser.password != that.chargebeeUser.cpassword){
                 this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Confirm Password not matched",
                    showConfirmButton: true
                });
            }
             else {
                that.chargebeeUser.disabled = true;
                const formData = new FormData();
                formData.append('companyname', that.chargebeeUser.companyname);
                formData.append('email', that.chargebeeUser.email);
                formData.append('domain', that.chargebeeUser.domain);
                formData.append('employees', that.chargebeeUser.employees);
                formData.append('plan', that.chargebeeUser.plan);
                formData.append('periodUnit', that.chargebeeUser.periodUnit);
                formData.append('planType', that.chargebeeUser.planType);
                formData.append('password', that.chargebeeUser.password);
                formData.append('addon', that.chargebeeUser.addon);
                formData.append('logo', that.chargebeeUser.logo);
                axios.post(APP_URL + '/create-company', formData,
                    {
                        headers: {
                            'Content-Type': 'multipart/form-data',
                            'Access-Control-Allow-Origin': '*'
                        }
                    }).then(response => {
                        // const elem = this.$refs.chargebeeBtn
                        // elem.click()
                        // that.chargebeeUser.disabled = false;
                        this.$swal({
                            icon: "success",
                            title: "Success",
                            text: "Company created successfully",
                            showConfirmButton: true
                        }).then(function () {
                            that.chargebeeUser.disabled = false;
                            if (that.modalAdd) {
                                that.$router.push('/admin/checkout/' + response.data.res.employee_registration_link);
                            } else {
                                that.$router.push('/checkout/' + response.data.res.employee_registration_link);
                            }
                        });

                    }
                    ).catch((error) => {
                        that.chargebeeUser.disabled = false;
                        this.$swal({
                            icon: "error",
                            title: "error",
                            text: error.response.data.message,
                            showConfirmButton: true
                        });
                    });
            }
        },
        getPlans: function () {
            let that = this;
            Api.getPlans2().then(response => {
                that.plans = response.data.res
            }
            ).catch((error) => {

            });
        },
    },
    created() {
        this.getPlans()
    }
}
</script>
