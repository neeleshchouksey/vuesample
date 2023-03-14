<script>
/* eslint-disable */
</script>
<template>
    <section class="employer-announcements-section half-cut-bg">
        <h3 class="page-title text-left mt-0">Feedback by <span>Employees</span></h3>

        <div class="table-responsive">
            <table class="table">
                <tr>
                    <th>Employee Name <i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'first_name'; searchData.sortOrder='asc';getFeedbackList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'first_name'; searchData.sortOrder='desc';getFeedbackList()"></i></th>
                    <th>Description<i class="fa-solid fa-arrow-up"
              @click="searchData.sortBy = 'description'; searchData.sortOrder='asc';getFeedbackList()"></i> <i
              class="fa-solid fa-arrow-down"
              @click="searchData.sortBy = 'description'; searchData.sortOrder='desc';getFeedbackList()"></i></th>
                </tr>
                <tr v-if="feedbackLength" v-for="f in feedbacks.data">
                    <td><span v-if="!f.anonymous">{{ f.first_name }} {{ f.last_name }}</span><span v-if="f.anonymous">Anonymous</span></td>
                    <td>{{ f.description }}</td>
                </tr>
                <tr v-if="!feedbackLength"><td> No data found</td></tr>
            </table>

        </div>
            <pagination :data="feedbacks" @pagination-change-page="getFeedbackList" />
    </section>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
export default {
    name: 'FeedbackEmployee',
    mixins: [AppMixin],
    data() {
        return {
            feedbacks: {},
            feedbackLength: 0,
            searchData:{
                'sortBy':'',
                'sortOrder':''
            }
        }
    },
    methods: {
        getFeedbackList: function (page = 1) {
            let that = this;
            Api.getFeedbackList(page,that.searchData).then(response => {
                that.feedbacks = response.data.res
                that.feedbackLength = that.feedbacks.data.length
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
    },
    mounted() {
        this.getFeedbackList()
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
