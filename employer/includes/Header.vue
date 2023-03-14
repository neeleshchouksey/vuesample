<template>
    <div class="header">
        <nav class="navbar navbar-expand-lg navbar-light ">
            <a class="navbar-brand" href="javascript:void(0)"><img src="../../../assets/images/logo.png"></a>
            <button class="navbar-toggler" data-bs-target="#employerSidebar">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ml-auto cutom-menu-list">
                    <li class="nav-item">
                        <a class="nav-link pd-link" href="#">Check-In</a>
                    </li>
                    <li class="nav-item">
                        <router-link class="nav-link pd-link" to="/employer/ask-question">Ask a question </router-link>
                    </li>
                    <li class="nav-item">
                        <router-link class="nav-link btn custom-btn" to="/employer/request-workshop">
                            <div class="gradient-btn">Request a Custom Workshop</div>
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


export default {
    name: "Header",
    props:['userProp'],
    data() {
        return {
        }
    },
    mixins: [AppMixin],
    methods: {
       
    },
    created() {

    },
}
</script>