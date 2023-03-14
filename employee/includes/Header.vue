<template>
    <div class="header">

            <nav class="navbar navbar-expand-lg navbar-light ">
                <router-link v-if="!company.company_logo" class="navbar-brand" to="/employee/dashboard"><img
                        src="../../../assets/images/logo.png">
                </router-link>
                <router-link v-if="company.company_logo" class="navbar-brand" to="/employee/dashboard"><img
                        :src="company.company_logo">
                </router-link>
                <button class="navbar-toggler" data-bs-target="#navbarSupportedContent">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="collapse navbar-collapse" id="navbarNav">
                    <ul class="navbar-nav ml-auto cutom-menu-list">
                        <li class="nav-item">
                            <router-link class="nav-link btn custom-btn" to="/employee/workshops">
                                <div class="gradient-btn">Register for a Workshop</div>
                            </router-link>
                        </li>
                    </ul>
                </div>
            </nav>
    </div>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'

export default {
    name: 'Header',
    props: ['userProp'],
    data() {
        return {
            authUser: {}
        }
    },
    mixins: [AppMixin],
    methods: {
        logout: function () {
            Api.logout().then(response => {
                window.localStorage.removeItem('token')
                window.localStorage.removeItem('userData')
                window.localStorage.removeItem('companyData')
                this.$router.go('/login')
            }
            ).catch((error) => {
                this.$swal({
                    icon: 'error',
                    title: 'error',
                    text: error.response.data.message,
                    showConfirmButton: true
                })
            })
        }
    },
    created() {
        this.authUser = this.userProp
        console.log(this.userProp)
    },
}
</script>
