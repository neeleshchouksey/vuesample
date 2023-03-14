<template>
    <b-modal id="add-resource-modal" title="Add New Resource" :hide-footer=hideFooter>
        <form enctype="multipart/form-data">
            <div id="details">
                <div class="form-group" v-if="user.role == 'ADMIN'">
                    <label>Select Company <span class="err">*</span></label>
                    <multiselect v-model="resource.company" :options="companiesListMultiselect" group-values="values"
                        group-label="selectAll" :multiple="true" :group-select="true" placeholder="Type to search"
                        track-by="name" label="name">
                        <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
                    </multiselect>
                </div>
                <div class="form-group">
                    <label>Title <span class="err">*</span></label>
                    <input type="text" class="form-control" id="title" placeholder="Title" v-model="resource.title">
                </div>
                <div class="form-group">
                    <label>Description<span class="err">*</span></label>
                    <textarea class="form-control" id="description" placeholder="Description"
                        v-model="resource.description"></textarea>
                </div>
                <div class="form-group">
                    <label>Link</label>
                    <input type="text" class="form-control" id="link" placeholder="Link" v-model="resource.link">
                </div>
                <div class="form-group">
                    <label>File</label>
                    <input type="file" class="form-control" id="file" ref="file" @change="fileOnChange">
                </div>
                <div class="form-group">
                    <label for="visibility" class="mx-2">Visibility<span class="err">*</span></label>
                    <br>
                    <input type="radio" id="PRIVATE" v-model="resource.visibility" value="PRIVATE">
                    <label for="PRIVATE" class="mx-2">PRIVATE</label>
                    <input type="radio" id="visibility2" v-model="resource.visibility" value="PUBLIC">
                    <label for="visibility2" class="mx-2">PUBLIC</label>
                </div>
                <button type="button" @click="addResource" class="btn btn-primary"
                    :disabled="resource.disabled">Submit</button>
            </div>
        </form>
    </b-modal>
</template>

<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
export default {
    name: 'Add',
    mixins: [AppMixin],
    props: {
        getResourcesList: {
            type: Function
        }
    },
    data() {
        return {
            hideFooter: true,
            resource: {
                'link': '',
                'company': '',
                'title': '',
                'description': '',
                'file': '',
                'visibility': '',
                'disabled': false,
            },
        }
    },
    methods: {
        fileOnChange: function (e) {
            let that = this;
            this.resource.file = this.$refs.file.files[0];
        },
        addResource: function (e) {
            e.preventDefault();
            let that = this;
            if (!that.resource.title || !that.resource.description) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill title and description fields",
                    showConfirmButton: true
                });
            } else if (that.resource.link == '' && that.resource.file == '') {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill link or upload file fields",
                    showConfirmButton: true
                });
            } else {
                that.resource.disabled = true;
                const formData = new FormData();
                formData.append('file', that.resource.file);
                formData.append('link', that.resource.link);
                formData.append('description', that.resource.description);
                formData.append('title', that.resource.title);
                formData.append('visibility', that.resource.visibility);
                if (that.user.role == "ADMIN") {
                    formData.append('company', JSON.stringify(that.resource.company));
                }
                Api.addResource(formData).then(response => {
                    that.resource.disabled = false;

                    let headers = {
                        'Content-Type': 'multipart/form-data',
                        'Access-Control-Allow-Origin': '*'
                    }
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Company resource created successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.$bvModal.hide('add-resource-modal')
                        that.resource.company = ''
                        that.resource.title = ''
                        that.resource.description = ''
                        that.resource.visibility = ''
                        that.resource.link = ''
                        that.$refs.file.value = null;
                        that.getResourcesList()
                    });
                }
                ).catch((error) => {
                    that.resource.disabled = false;
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.resource.disabled = false;
                    });
                });
            }
        },


    },
    mounted() {
        this.getCompaniesListMultiselect()
    }
}
</script>
