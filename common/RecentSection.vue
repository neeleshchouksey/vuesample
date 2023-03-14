<template>
    <div>
        <div class="mb-3">
            <div class="card home-side-card">
                <div class="card-body">
                    <h5 class="card-title">
                        <a href="javascript:void(0)">
                            Chats
                        </a>
                    </h5>
                    <router-link v-for="cl in chatList" v-bind:key="cl.id"
                        :to="'/' + currentUrl + '/one-to-one-chat/' + cl.id" v-if="chatList.length"
                        class="dashboard-card-link">
                        {{ cl.first_name }} {{ cl.last_name }}
                    </router-link>
                </div>
            </div>
        </div>
        <div class="mb-3" v-if="company.role == 'COMPANY_ADMIN'">
            <div class="card home-side-card mt-0">
                <div class="card-body">
                    <h5 class="card-title">
                        <router-link :to="'/' + currentUrl + '/request-workshop'">
                            Requested Workshops <img src="../../assets/images/chevron-right.svg" alt="arrow-right" />
                        </router-link>
                    </h5>
                    <router-link :to="'/' + currentUrl + '/request-workshop'" v-if="requestedWorkshopList.length"
                        v-for="rw in requestedWorkshopList" v-bind:key="rw.id" class="dashboard-card-link">
                        {{ rw.name }}
                    </router-link>
                </div>
            </div>
        </div>
        <div class="mb-3">
            <div class="card home-side-card">
                <div class="card-body">
                    <h5 class="card-title">
                        <router-link :to="'/' + currentUrl + '/workshops'">
                            Workshops <img src="../../assets/images/chevron-right.svg" alt="arrow-right" />
                        </router-link>
                    </h5>
                    <router-link v-if="currentUrl == 'employer' && workshopList.length" v-for="rw in workshopList"
                        :to="'/' + currentUrl + '/view-workshop/' + rw.id" v-bind:key="rw.id"
                        class="dashboard-card-link">
                        {{ rw.title }}
                    </router-link>
                    <router-link v-if="currentUrl == 'employee' && workshopList.length" v-for="rw in workshopList"
                        :to="'/' + currentUrl + '/workshop/' + rw.id" v-bind:key="rw.id" class="dashboard-card-link">
                        {{ rw.title }}
                    </router-link>
                </div>
            </div>
        </div>
        <!-- card end -->
        <div class="mb-3">
            <div class="card home-side-card">
                <div class="card-body">
                    <h5 class="card-title">
                        <router-link :to="'/' + currentUrl + '/announcements'">
                            Announcements <img src="../../assets/images/chevron-right.svg" alt="arrow-right" />
                        </router-link>
                    </h5>
                    <router-link :to="'/' + currentUrl + '/announcements'" v-if="announcementsList.length"
                        v-for="rw in announcementsList" v-bind:key="rw.id" class="dashboard-card-link">
                        {{ rw.title }}<br>
                    </router-link>
                </div>
            </div>
        </div>
        <!-- <div class="mb-3">
            <div class="card home-side-card">
                <div class="card-body">
                    <h5 class="card-title">
                        <router-link :to="'/' + currentUrl + '/resources'">
                            Resources <img src="../../assets/images/chevron-right.svg" alt="arrow-right" />
                        </router-link>
                    </h5>
                    <router-link :to="'/' + currentUrl + '/resources'" v-if="resourcesList.length"
                        v-for="rw in resourcesList" v-bind:key="rw.id" class="dashboard-card-link">
                        {{ rw.title }}
                    </router-link>
                </div>
            </div>
        </div> -->
    </div>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
export default {
    name: 'Dashboard',
    mixins: [AppMixin],
    data() {
        return {
            resourcesList: [],
            announcementsList: [],
            workshopList: [],
            requestedWorkshopList: [],
            chatList: [],
            currentUrl: ''
        }
    },
    methods: {
        getRecentAnnouncementsList: function () {
            let that = this
            Api.getRecentAnnouncementsList(that.company.id).then(response => {
                let that = this
                that.announcementsList = response.data.res
            }
            ).catch((error) => {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: error.response.data.message,
                    showConfirmButton: true
                });
            });
        },
        getRecentRequestedWorkshopList: function () {
            let that = this
            Api.getRecentRequestedWorkshopList(that.company.id).then(response => {
                let that = this
                that.requestedWorkshopList = response.data.res
            }
            ).catch((error) => {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: error.response.data.message,
                    showConfirmButton: true
                });
            });
        },
        getRecentWorkshopList: function () {
            let that = this
            Api.getRecentWorkshopList(that.company.id).then(response => {
                let that = this
                that.workshopList = response.data.res
            }
            ).catch((error) => {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: error.response.data.message,
                    showConfirmButton: true
                });
            });
        },
        getRecentResourceList: function () {
            let that = this
            Api.getRecentResourceList(that.company.id).then(response => {
                let that = this
                that.resourcesList = response.data.res
            }
            ).catch((error) => {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: error.response.data.message,
                    showConfirmButton: true
                });
            });
        },
        getRecentChatList: function () {
            let that = this
            Api.getRecentChatList(that.company.id).then(response => {
                let that = this
                that.chatList = response.data.res
            }
            ).catch((error) => {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: error.response.data.message,
                    showConfirmButton: true
                });
            });
        },
    },
    created() {
        var url = document.URL.split('/');
        this.currentUrl = url[3]
        console.log(url)
        this.getRecentAnnouncementsList()
        this.getRecentChatList()
        this.getRecentRequestedWorkshopList()
        this.getRecentWorkshopList()
        this.getRecentResourceList()
    }
}
</script>
