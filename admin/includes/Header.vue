<template>
    <div class="header">
        <nav class="navbar navbar-expand-lg navbar-light ">
            <a class="navbar-brand" href="javascript:void(0)"><img src="../../../assets/images/logo.png"></a>
            <button class="navbar-toggler" data-bs-target="#adminSidebar">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ml-auto cutom-menu-list">
                    <!-- <li class="nav-item">
                        <a class="nav-link pd-link" href="#">Check-In</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link pd-link" href="#">Ask a question </a>
                    </li> -->
                    <li class="nav-item">
                        <b-nav-item-dropdown right no-caret >
                            <template slot="button-content">
                                <span class="notification" v-if="unseen">{{unseen}}</span>
                                <i class="py-0 fa fa-bell" @click="readAdminNotifications"></i>
                            </template>
                            <b-dropdown-item href="javascript::void(0)" v-for="nl in notificationList" v-bind:key="nl.id">{{nl.notification}}</b-dropdown-item>
                        </b-nav-item-dropdown>
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
    name: "Header",
    props: ['userProp'],
    data() {
        return {
            authUser: {},
            notificationList: [],
            unseen: 0
        }
    },
    mixins: [AppMixin],
    methods: {
        getAdminNotifications: function () {
            let that = this
            Api.getAdminNotifications().then(response => {
                that.notificationList = response.data.res
                that.unseen = response.data.unseen
            }
            ).catch((error) => {
                this.$swal({
                    icon: 'error',
                    title: 'error',
                    text: error.response.data.message,
                    showConfirmButton: true
                })
            })
        },
        readAdminNotifications: function () {
            let that = this
            Api.readAdminNotifications().then(response => {
                that.unseen = 0
            }
            ).catch((error) => {
                this.$swal({
                    icon: 'error',
                    title: 'error',
                    text: error.response.data.message,
                    showConfirmButton: true
                })
            })
        },
    },
    created() {
        this.getAdminNotifications();
        this.authUser = this.userProp
        window.Echo.channel('notification' + this.user.id)
            .listen('AdminNotificationEvent', (e) => {
                console.log('Event calling' + 'MessageSent' + this.user.id)
                this.getAdminNotifications();
            })
    },
}
</script>