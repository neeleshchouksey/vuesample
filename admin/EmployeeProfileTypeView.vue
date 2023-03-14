<template>
    <section class="popup-survey-view-section half-cut-bg">
        <router-link to="/admin/employee-profile-type" class="btn back">
            <!-- <button class="btn-primary"> -->
            <img src="../../assets/images/arrow-left.svg" alt="arrow-left" /> Back
            <!-- </button> -->
        </router-link>
        <h3 class="page-title text-left mt-0">Employee <span>Profile Type Details</span></h3>
        <p><b>Profile Type:</b> <span class="links">{{ profileType.profile_type }} </span></p>
        <p><b>File:</b> <a class="cursor-pointer links"
                @click="downloadProfileTypeFile(profileType.id, profileType.file)">{{
        profileType.file
                }}</a></p>
        <div class="row mt-5">
            <Section1></Section1>
            <Section2></Section2>
            <Section3></Section3>
        </div>
    </section>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import { Vue2TinymceEditor } from "vue2-tinymce-editor";
import Section1 from "./Section1.vue";
import Section2 from "./Section2.vue";
import Section3 from './Section3.vue';

export default {
    name: 'EmployeeProfileTypeView',
    mixins: [AppMixin],
    data() {
        return {
            profileType: {}
        }
    },
    components: {
        Vue2TinymceEditor,
        Section1,
        Section2,
        Section3
    },
    methods: {

        getProfileType: function () {
            let that = this
            Api.getProfileType(this.$route.params.id).then(response => {
                that.profileType = response.data.res
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
        this.getProfileType()
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
