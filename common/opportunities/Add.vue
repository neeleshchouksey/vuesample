<template>
    <b-modal id="add-opportunity-modal" size="lg" title="Add New Opportunity" :hide-footer=hideFooter no-fade
        no-enforce-focus>
        <form>
            <div class="form-group">
                <label>Select Company <span class="err">*</span></label>
                <multiselect v-model="opportunity.company" :options="companiesListMultiselect" group-values="values"
                    group-label="selectAll" :multiple="true" :group-select="true" placeholder="Type to search"
                    track-by="name" label="name">
                    <span slot="noResult">Oops! No elements found. Consider changing the search query.</span>
                </multiselect>
            </div>
            <div class="form-group">
                <label>Content <span class="err">*</span></label>
                <vue2-tinymce-editor v-model="opportunity.description" placeholder="Content">
                </vue2-tinymce-editor>
            </div>
            <div class="form-group">
                <button type="button" class="btn btn-primary" @click="addOpportunity"
                    :disabled="opportunity.disabled">Submit</button>
            </div>
        </form>
    </b-modal>
</template>
<style src="vue-multiselect/dist/vue-multiselect.min.css">
</style>
<script>
/* eslint-disable */
import AppMixin from '../../../mixins/AppMixin'
import Api from '../../../router/api'
import { Vue2TinymceEditor } from "vue2-tinymce-editor";

export default {
    name: 'Add',
    mixins: [AppMixin],
    components: {
        Vue2TinymceEditor
    },
    props: {
        getOpportunityList: {
            type: Function
        }
    },
    data() {
        return {
            hideFooter: true,
            opportunity: {
                company: '',
                description: '',
                disabled: false
            },
            companiesList: [
                {
                    selectAll: 'Select All',
                    values: []
                }
            ],
        }
    },
    methods: {
        addOpportunity: function () {
            let that = this;
            if (!that.opportunity.description || !that.opportunity.company) {
                this.$swal({
                    icon: "error",
                    title: "error",
                    text: "Please fill all required fields",
                    showConfirmButton: true
                })
            } else {
                that.opportunity.disabled = true;
                Api.addOpportunity(that.opportunity).then(response => {
                    that.opportunity.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Opportunity added successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.opportunity.disabled = false
                        that.opportunity.description = ''
                        that.opportunity.company = ''
                        that.$bvModal.hide('add-opportunity-modal')
                        that.getOpportunityList()
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.opportunity.disabled = false;
                    });
                });
            }
        },
        getCompaniesList: function () {
            let that = this
            Api.getCompaniesList().then(response => {
                let that = this
                that.companiesList[0].values = response.data.res
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
    mounted() {
        this.getCompaniesListMultiselect()
    }
}
</script>
