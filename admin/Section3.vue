<template>
    <div class="col-md-4">
        <h2 class="blue-color"><strong>Section 3</strong></h2>
        <form enctype="multipart/form-data" @submit="addUpdateSection3">
            <div class="form-group">
                <label>Title</label>
                <input class="form-control" type="text" placeholder="title" v-model="section3.title" />
            </div>
            <div class="form-group">
                <label>Description</label>
                <vue2-tinymce-editor v-model="section3.description" placeholder="Description" id="description3">
                </vue2-tinymce-editor>

            </div>
            <div class="form-group form-group pt-4 mt-4">
                <button type="submit" class="btn btn-primary">Submit</button>
            </div>
        </form>
    </div>

</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
import { Vue2TinymceEditor } from "vue2-tinymce-editor";

export default {
    name: 'EmployeeProfileTypeView',
    mixins: [AppMixin],
    data() {
        return {
            profileType: {}
        }
    },
    components: {
        Vue2TinymceEditor
    },
    methods: {
        addUpdateSection3: function (e) {
            e.preventDefault()
            let that = this;
            console.log(that.section1);
            if (!that.section3.title || !that.section3.description) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required  fields",
                    showConfirmButton: true
                })
            } else {
                that.section3.disabled = true;
                const formData = new FormData();
                // formData.append('image', that.section3.image);
                formData.append('title', that.section3.title);
                formData.append('description', that.section3.description);
                formData.append('profileType', that.$route.params.id);
                formData.append('id', that.section3.id);
                Api.addUpdateSection3(formData).then(response => {
                    that.section3.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Updated successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.section3.disabled = false
                        that.$refs.imageMultiple.value = null;
                        that.getSection3()
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.section3.disabled = false;
                    });
                });
            }
        },
    },
    mounted() {
        this.getSection3()
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
