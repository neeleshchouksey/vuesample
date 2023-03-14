<template>
    <section class="check-in-survey-view-section half-cut-bg">
        <router-link to="/admin/check-in-surveys" class="btn back">
            <!-- <button class="btn-primary"> -->
            <img src="../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
            <!-- </button> -->
        </router-link>
        <h1 class="page-title text-left mt-0">Check-In <span>Survey Answers</span></h1>
        <h2 class="page-sub-title">{{ survey.question }}</h2>
        <div class="row mt-3">
            <table class="table">
                <tr>
                    <th>Company Name</th>
                    <th>Employee Name</th>
                    <th>Answer</th>
                </tr>
                <tr v-if="checkInSurveyLength" v-for="p in checkInSurvey.data" v-bind:key="p.id">
                    <td>{{ p.company_name }}</td>
                    <td>{{ p.first_name }} {{ p.last_name }}</td>
                    <td>{{ p.answer }}</td>
                </tr>
                <tr v-if="!checkInSurveyLength">
                    <td colspan="5">No Data Found</td>
                </tr>
            </table>
        </div>
    </section>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
export default {
    name: 'CheckInSurveyView',
    mixins: [AppMixin],
    data() {
        return {
            checkInSurvey: {},
            checkInSurveyLength: 0,
            survey: {}
        }
    },
    components: {
    },
    methods: {
        getCheckInSurvey: function (id) {
            let that = this
            Api.getCheckInSurvey(id).then(response => {
                that.survey = response.data.res
            }).catch((error) => {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: error.response.data.message,
                    showConfirmButton: true
                }).then(function () {
                });
            });

        },
        getCheckInSurveyAnswerList: function (id) {
            let that = this
            Api.getCheckInSurveyAnswerList(id).then(response => {
                that.checkInSurvey = response.data.res
                that.checkInSurveyLength = that.checkInSurvey.data.length
            }).catch((error) => {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: error.response.data.message,
                    showConfirmButton: true
                }).then(function () {
                });
            });

        }
    },
    mounted() {
        this.getCheckInSurvey(this.$route.params.id)
        this.getCheckInSurveyAnswerList(this.$route.params.id)
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
