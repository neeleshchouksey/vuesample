<template>
    <section class="popup-survey-view-section half-cut-bg">
        <router-link to="/admin/popup-surveys" class="btn back">
            <!-- <button class="btn-primary"> -->
            <img src="../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
            <!-- </button> -->
        </router-link>
        <h1 class="page-title text-left mt-0">Popup Survey Answers</h1>
        <h2 class="page-sub-title">{{ survey.question }}</h2>
      <div class="table-responsive">
            <table class="table">
                <tr>
                    <th>Company Name</th>
                    <th>Employee Name</th>
                    <th>Answer</th>
                </tr>
                <tr v-if="popupSurveyLength" v-for="p in popupSurvey.data" v-bind:key="p.id">
                    <td>{{ p.company_name }}</td>
                    <td>{{ p.first_name }} {{ p.last_name }}</td>
                    <td>{{ p.answer }}</td>
                </tr>
                <tr v-if="!popupSurveyLength">
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
    name: 'PopupSurveyView',
    mixins: [AppMixin],
    data() {
        return {
            popupSurvey: {},
            popupSurveyLength: 0,
            survey: {}
        }
    },
    components: {
    },
    methods: {
        getPopupSurvey: function (id) {
            let that = this
            Api.getPopupSurvey(id).then(response => {
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
        getPopupSurveyAnswerList: function (id) {
            let that = this
            Api.getPopupSurveyAnswerList(id).then(response => {
                that.popupSurvey = response.data.res
                that.popupSurveyLength = that.popupSurvey.data.length
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
        this.getPopupSurvey(this.$route.params.id)
        this.getPopupSurveyAnswerList(this.$route.params.id)
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
