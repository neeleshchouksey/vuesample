<template>
    <div class="col-md-9">
        <h3>Send Email</h3>
        <button @click="sendEmail" class="btn btn-primary" :disabled="disabled">Send Email</button>
    </div>
</template>
<script>
/* eslint-disable */
import AppMixin from '../../mixins/AppMixin'
import Api from '../../router/api'
export default {
    name: 'Todo',
    mixins: [AppMixin],
    data() {
        return {
            disabled:false
        }
    },
    methods: {
        sendEmail: function(){
            let that = this
            that.disabled = true
                 Api.sendEmail(that.addData).then(response => {
                    that.disabled = false;
                    this.$swal({
                        icon: "success",
                        title: "Success",
                        text: "Email sent successfully",
                        showConfirmButton: true
                    }).then(function () {
                        that.disabled = false
                    });
                }).catch((error) => {
                    this.$swal({
                        icon: "error",
                        title: "error",
                        text: error.response.data.message,
                        showConfirmButton: true
                    }).then(function () {
                        that.disabled = false;
                    });
                });
        }
    },
    mounted() {
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
