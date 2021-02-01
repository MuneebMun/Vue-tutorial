<template>
  <div id="employee-list-container">
    <employee-form @onAdd:employee='addEmployee'/>
     <h3>Employee List</h3>
    
    <employee-list 
      :employeeDetails="employeeDetails"
      :isLoading="isLoading" 
      @on-delete-employee="deleteEmployee"
      @edit-employee-details="editEmployeeDetail"
    
    />
    <!-- this is the same thing -->
    <!-- <employee-table v-bind:employeeDetails="employeeDetails" /> -->
  </div>
</template>

<script>
import EmployeeForm from './components/EmployeeForm.vue'
import EmployeeList from './components/EmployeeList.vue'

export default {
  name: 'App',
  // all the imported component must be added here
  components: {
    EmployeeList,
    EmployeeForm,
  },
  methods: {
    addEmployee: function(newEmployeeDetail) {
      // assign new ids 
      const getLastId = this.employeeDetails.length > 0 ? this.employeeDetails[this.employeeDetails.length -1].id : 0;
      const id = getLastId + 1;
      const newEmployee = {...newEmployeeDetail, id}
      this.employeeDetails = [...this.employeeDetails, newEmployee];
    },
    editEmployeeDetail: function (employeeDetail) {
      console.log(employeeDetail,"HELLO")
    },
    deleteEmployee: function(id) {
      this. employeeDetails = this.employeeDetails.filter(employeeDetail => employeeDetail.id !== id)
    }
  },
  data () {
    return {
      employeeDetails : [],
      isLoading: true,
    }
  },

  async mounted(){
    try {
       const response = await fetch ("https://jsonplaceholder.typicode.com/users")
       const data = await response.json();
       this.employeeDetails = data;
     }
     catch(err) {
       console.log(err)
     }
     finally {
       // DELAYING JUST TO GET SENSE OF THE LODING SPINNER
       setTimeout(() => {
         this.isLoading = false;
       },4000)
      
     }
  }

}
</script>

<style>

  .employee-list-container {
    max-width: 680px;
  }
</style>