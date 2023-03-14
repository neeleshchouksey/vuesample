<template>
  <section class="member-ship-details-section half-cut-bg">
    <h1 class="page-title text-left ">Membership <span>Details</span></h1>
      <div class="row" v-for="pd in planDetails" v-bind:key="pd.item_price_id">
        <div class="col-md-4">
          <div class="card-box my-3">
          <p> Name : <span class="pink-color"> {{ pd.item_price_id }} </span></p>
          <p> Type : <span class="pink-color"> {{ pd.item_type }} </span></p>
          <p> Price : <span class="pink-color"> {{ pd.amount / 100 }} </span></p>
          <p> Consulting Hours Total : <span class="pink-color" v-if="pd.item_type == 'plan'"> Consulting Hours Total: {{company.total_hours}}  </span></p>
          <p> Consulting Hours Remaining: <span class="pink-color" v-if="pd.item_type == 'plan'"> {{company.remaining_hours}} </span></p>
        </div>
      </div>
      </div>
    <div class="row" v-if="company.plan_type!='One-Time-Users-Plan'">
      <div class="col-md-6">
        <div class="form-group">
          <select type="text" class="form-control" id="addon" placeholder="Addon" v-model="chargebeeUser.addon">
            <option v-for="plan in addons" v-bind:value="plan.id" v-bind:key="plan.id">{{ plan.name }}
            </option>
          </select>
        </div>
        <div class="form-group">
          <button type="button" class="btn btn-primary" @click="selectAddon">Submit</button>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
/* eslint-disable */

import Api from '../../router/api'
import AppMixin from '../../mixins/AppMixin'
export default {
  name: 'MembershipDetails',
  mixins: [AppMixin],
  data() {
    return {
      subscriptionId: '',
      planDetails: [],
      addons: [],
      chargebeeUser: {
        addon: '',
        disabled: false
      }
    }
  },
  methods: {
    getPlanDetailsBySubscriptionId: function () {
      let that = this
      Api.getPlanDetailsBySubscriptionId(that.company.chargebee_subscription_id).then(response => {
        that.planDetails = response.data.res
      })
    },
    getAddons: function () {
      let that = this;
      Api.getAddons().then(response => {
        that.addons = response.data.res
      }
      ).catch((error) => {

      });
    },
    selectAddon: function () {
      let that = this;
      Api.selectAddon(that.company.chargebee_subscription_id,that.chargebeeUser).then(response => {
        this.$swal({
          icon: "success",
          title: "Success",
          text: "Addon selected successfully",
          showConfirmButton: true
        }).then(function () {
          that.chargebeeUser.disabled = false;
          that.chargebeeUser.addons = ''
          window.location = response.data.res.url
        });

      }
      ).catch((error) => {

      });
    }
  },
  mounted() {
    if (this.company.chargebee_subscription_id) {
      this.getPlanDetailsBySubscriptionId()
    }
    this.getAddons()

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

</style>
