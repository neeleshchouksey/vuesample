<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6 offset-3">
        <h1>{{ msg }}</h1>

        <form>
          <p class="err" v-if="errors">
            {{ errors }}
          </p>
          <div v-if="showDiv" id="details">
           <div class="form-group">
            <label>Company Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="companyname" placeholder="Company Name"
              v-model="chargebeeUser.companyname" @focus="removeError">
          </div>
          <div class="form-group">
            <label>Email <span class="err">*</span></label>
            <input type="email" class="form-control" id="email" aria-describedby="emailHelp" placeholder="Email"
              v-model="chargebeeUser.email" @focus="removeError">
          </div>
          <div class="form-group">
            <label>First Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="firstname" placeholder="First Name"
              v-model="chargebeeUser.firstname" @focus="removeError" @keypress="alphabetsOnly">
          </div>

          <div class="form-group">
            <label>Last Name <span class="err">*</span></label>
            <input type="text" class="form-control" id="lastname" placeholder="Last Name"
              v-model="chargebeeUser.lastname" @focus="removeError" @keypress="alphabetsOnly">
          </div>
          <div class="form-group">
            <label>Address <span class="err">*</span></label>
            <input type="text" class="form-control" id="address" placeholder="Address" v-model="chargebeeUser.address"
              @focus="removeError">
          </div>
          <div class="form-group">
            <label>City <span class="err">*</span></label>
            <input type="text" class="form-control" id="city" placeholder="City" v-model="chargebeeUser.city"
              @focus="removeError">
          </div>
          <div class="form-group">
            <label>State <span class="err">*</span></label>
            <input type="text" class="form-control" id="state" placeholder="State" v-model="chargebeeUser.state"
              @focus="removeError">
          </div>
          <div class="form-group">
            <label>Zip Code <span class="err">*</span></label>
            <input type="text" class="form-control" id="zip" placeholder="Zip Code" v-model="chargebeeUser.zip"
              @focus="removeError">
          </div>
          <button type="button" @click="nextStep" class="btn btn-primary">Next</button>
          </div>
          <div v-if="!showDiv" id="plan">
          <div class="form-group">
            <label>Number of employees <span class="err">*</span></label>
            <input type="text" class="form-control" id="employees" placeholder="Number of employees"
              v-model="chargebeeUser.employees" @focus="removeError">
          </div>
          <div class="form-group">
            <label>Select Plan <span class="err">*</span></label>
            <select type="text" class="form-control" id="plan" placeholder="Plan"
              v-model="chargebeeUser.plan" @focus="removeError" v-on:change="getSubscriptionPlans">
              <option v-for="plan in plans" v-bind:value="plan.id" v-bind:key="plan.id">{{plan.plan_name}}</option>
              </select>
          </div>
           <div class="form-group" v-show="subscriptionPlans.length">
            <label>Select Subscription Plan <span class="err">*</span></label>
            <select type="text" class="form-control" id="subscriptionplan" placeholder="Subscription Plan"
              v-model="chargebeeUser.subscriptionplan" @focus="removeError">
              <option v-for="plan in subscriptionPlans" v-bind:value="plan.id" v-bind:key="plan.id">{{plan.subscription_plan_name}}</option>
              </select>
          </div>
          <button type="button" @click="showDiv=true" class="btn btn-primary">Pre</button>
          <button type="button" @click="validateForm" class="btn btn-primary">Submit</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
/* eslint-disable */
export default {
  name: 'Chargebee',
  data() {
    return {
      errors: '',
      msg: 'Chargebee',
      chargebeeUser: {
        'companyname':'',
        'firstname': '',
        'lastname': '',
        'email': '',
        'address': '',
        'city': '',
        'state': '',
        'zip': '',
        'disabled': false,
        'employees':'',
        'plan':'',
        'subscriptionplan':''
      },
      showDiv:true,
      plans:[],
      subscriptionPlans:[]
    }
  },
  methods: {
    validateForm: function (e) {
      e.preventDefault();
      let that = this;
     
        axios.post(APP_URL + '/create-customer', that.chargebeeUser).then(response => {
          that.chargebeeUser.disabled = true;
          this.$swal({
            icon: "success",
            title: "Success",
            text: "Customer created successfully",
            showConfirmButton: true
          }).then(function () {
            that.chargebeeUser.disabled = false;
          });
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
    removeError: function () {
      let that = this;
      that.errors = '';
    },
    nextStep: function(e){
      e.preventDefault;
      console.log("next step")
      let that = this;
       if (!that.chargebeeUser.companyname || !that.chargebeeUser.email || !that.chargebeeUser.firstname || !that.chargebeeUser.lastname || !that.chargebeeUser.address || !that.chargebeeUser.city || !that.chargebeeUser.state || !that.chargebeeUser.zip) {
        this.$swal({
          icon: "error",
          title: "error",
          text: "Please fill all required fields",
          showConfirmButton: true
        });
      } else{
      that.showDiv = false;
      that.getPlans();
      }
    },
    getPlans: function(){
      let that = this;
      axios.get(APP_URL + '/get-plans').then(response => {
          that.plans = response.data.res
        }
        ).catch((error) => {
         
        });
    },
    getSubscriptionPlans: function(){
      let that = this;
      let id = that.chargebeeUser.plan
      axios.get(APP_URL + '/get-subscription-plans/'+id).then(response => {
          that.subscriptionPlans = response.data.res
        }
        ).catch((error) => {
         
        });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.form-group {
  margin-bottom: 1rem;
}

.err {
  color: red;
}
</style>
