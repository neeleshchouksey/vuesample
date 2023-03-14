<template>
    <div class="header">
        <nav class="navbar navbar-expand-lg navbar-light ">
            <a class="navbar-brand" href="javascript:void(0)"><img src="../../../assets/images/logo.png"></a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav"
                aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ml-auto cutom-menu-list">
                    <li class="nav-item">
                        <router-link class="nav-link pd-link" to="/employee/feedback-to-company">Feedback to Company</router-link>
                    </li>
                    <li class="nav-item">
                        <router-link class="nav-link pd-link" to="/employee/ask-question">Ask a question </router-link>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link btn custom-btn" href="#">
                            <div class="gradient-btn">Register for a workshop</div>
                        </a>
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
            authUser:{}
        }
    },
    mixins: [AppMixin],
    methods: {
       
    },
    created() {
        this.authUser = this.userProp
        console.log(this.userProp)
    },
}
</script>