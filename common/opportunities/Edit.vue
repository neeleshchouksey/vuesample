<template>
    <b-modal id="update-opportunity-modal" size="lg" title="Update Opportunity" :hide-footer=hideFooter no-fade
        no-enforce-focus>
        <form>
            <div class="form-group">
                <label>Select Company <span class="err">*</span></label>
                <multiselect v-model="opportunityUpdate.company" :options="companiesListMultiselect"
                    group-values="values" group-label="selectAll" :multiple="true" :group-select="true"
                    placeholder="Type to search" track-by="name" label="name">
                    <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
                </multiselect>
            </div>
            <div class="form-group">
                <label>Content <span class="err">*</span></label>
                <vue2-tinymce-editor v-model="opportunityUpdate.description" placeholder="Content">
                </vue2-tinymce-editor>
            </div>
            <div class="form-group">
                <button type="button" class="btn btn-primary" @click="updateOpportunity"
                    :disabled="opportunityUpdate.disabled">Submit</button>
            </div>
        </form>
    </b-modal>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import { Vue2TinymceEditor } from "vue2-tinymce-editor";

export default {
    name: 'Edit',
    mixins: [AppMixin],
    props: [
        'getOpportunityList',
        'opportunityUpdate'
    ],
    components: {
        Vue2TinymceEditor
    },
    data() {
        return {
            hideFooter: true,
        }
    },
    methods: {
        updateOpportunity: function () {
            let that = this;
            if (!that.opportunityUpdate.description || !that.opportunityUpdate.company) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required fields",
                    showConfirmButton: true
                });
            } else {
                that.opportunityUpdate.disabled = true
                Api.updateOpportunity(that.opportunityUpdate).then(response => {
                    that.opportunityUpdate.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Opportunity updated successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.opportunityUpdate.disabled = false
                        that.$bvModal.hide('update-opportunity-modal')
                        that.getOpportunityList()
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.opportunityUpdate.disabled = false;
                    });
                });
            }
        },
    },
    mounted() {
        this.getCompaniesListMultiselect()
    },
    created() {
        console.log(this.opportunityUpdate)
    }
}
</script>
