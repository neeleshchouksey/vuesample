
<script>
import Api from '../router/api'
/* eslint-disable */
export default {
  methods: {
    logout_from_assessment: function () {
      
        window.localStorage.removeItem('token')
        window.localStorage.removeItem('userData')
        window.localStorage.removeItem('companyData')
        //this.myWindow.close();  
      
    },
  },
  mounted() {
    this.logout_from_assessment();
    window.close('','_parent','');
  }
}
</script>
