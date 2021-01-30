<template>
<div id="container">
    <table id="employee-details">
      <thead>
        <tr>
          <th>Employee ID</th>
          <th>Employee name</th>
          <th>Employee email</th>
          <th>Actions</th>
          
        </tr>
      </thead>
      <tbody>
          <tr v-for="employee in employeeDetails" :key="employee.id" data-name="employee.id">
              <td>{{employee.id}}</td>
              
              <td v-if="employeeID === employee.id">
                 <input type="text" v-model="employee.name"/>
              </td>
              <td  v-else>{{employee.name}}</td>


              <td v-if="employeeID === employee.id">
                 <input type="text"  v-model="employee.email" />
              </td>
              <td v-else>{{employee.email}}</td>
              <td v-if="employeeID=== employee.id">
                <button class="custom-button" v-on:click="onSave(employee)"> Save </button>
                <button class="cancel-button" v-on:click="onCancel(employee)"> Cancel </button>
              </td>

              <td v-else>
                 <button  class="custom-button" v-on:click="onEditEmployeeDetails(employee)"> Edit </button>
                <button class="delete-button" v-on:click="onDeleteEmployee(employee.id)">Delete</button>
              </td>
          </tr>
      </tbody>
    </table>
</div>
</template>

<script>
export default {
    name: 'EmployeeList',
    // mention what kind of props you are expecting 
    props: {
        employeeDetails: Array //  so we tell the component that it will receive props, in this case an Array
    },
    mounted(){
        console.log(this.employeeDetails)
    },
    data: function () {
      return {
        employeeID: null
      }
    },
    methods: {
      onDeleteEmployee: function(id) {
        //emit event and pass to parent 
        this.$emit('on-delete-employee', id)
      },
      onEditEmployeeDetails: function(employee) {
        this.cachedEmployees = Object.assign({},employee)
        this.employeeID = employee.id
      },
      onSave:function(employeeDetail) {
        this.$emit('edit-employee-details', employeeDetail)
        this.employeeID = null;
      },
      onCancel: function(employee) {
        console.log(employee,"UPDATED")
        // TO CANCEL UPDATE FROM UPDATING WE NEED TO CACHE THE OBJECT. 
        Object.assign(employee, this.cachedEmployees)
        this.employeeID = null;
      }
    }
}
</script>

<style scoped>
#employee-details { 
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#employee-details td, #employee-details th {
  border: 1px solid #ddd;
  padding: 8px;
  
}



#employee-details th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
}

.delete-button {
   background-color: #f44336;
   color: white;
   padding: 6px 14px;
   cursor: pointer;
}

.custom-button {
  background-color: #4CAF50;
  color: white;
  padding: 6px 14px;
  cursor: pointer;
  margin-right: 14px;

}

.cancel-button {
  padding: 6px 14px;
  cursor: pointer;
  margin-right: 14px;
}

input {
  padding: 12px 20px;
    margin: 8px 0;
    box-sizing: border-box;
}

</style>