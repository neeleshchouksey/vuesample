<template>
    <section class="login-section">
        <div class="login-inner">
            <div class="login-box">
                <!-- <h5>You Payment Completed Successfully</h5>
                <p>
                    Please <router-link to="/login">Login</router-link> to continue
                </p> -->
                <b>Loading...</b>
            </div>
        </div>
    </section>
</template>

<script>
// import Api from '../router/api'
/* eslint-disable */
import Api from '../router/api'
import AppMixin from '../mixins/AppMixin'
export default {
    name: 'CreateCompany',
    mixins: [AppMixin],
    data() {
        return {

        }
    },
    methods: {
        updatePaymentStatus: function () {
            Api.updatePaymentStatus(this.$route.params.link).then(response => {
                if (response.data.company) {
                    window.localStorage.setItem('token', response.data.access_token)
                    window.localStorage.setItem('userData', JSON.stringify(response.data.user))
                    window.localStorage.setItem('companyData', JSON.stringify(response.data.company))
                    if (response.data.company.role == 'COMPANY_ADMIN') {
                        window.location = '/employer/dashboard'
                    } else {
                        window.location = '/employee/dashboard'
                    }
                } else {
                    // if (response.data.user) {
                    //     window.location = '/admin/dashboard'
                    // } else {
                         window.location = '/admin/companies'
                    // }
                }
            })
        }
    },
    mounted() {
        this.updatePaymentStatus()
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form-group {
    margin-bottom: 1rem;
}

.err {
    color: red;
}
</style>
