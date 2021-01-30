<template>
   <div>
       <p v-if="displayErrorMessage" class="error-message">❗Please fill out all required fields</p>
       <p v-if="displaySuccessMessage" class="success-message">✅ Employee successfully added </p>
    <div id="employee-form">
        <label>Employee ID</label>
        <input class="employee-info" v-model="newEmployeeDetail.id" type="text"/>
        <label>Employee Name</label>
        <input type="text" class="employee-info" v-model="newEmployeeDetail.name"/>
        <label>Employee Email</label>
        <input type="text" class="employee-info" v-model="newEmployeeDetail.email"/>
         <button v-on:click="onAddEmployee"> Add Employee </button>
    </div>
    </div>
</template>

<script>
export default {
    name: 'employee-form',
    // A Component’s data option must be a function, so that each instance can maintain an independent copy of the returned data object:
    data: function() {
        return {
             newEmployeeDetail: {
                id: '',
                name: '',
                email: ''
            },
            displayErrorMessage: false,
            displaySuccessMessage: false,
        }
    },
    methods: {
        invalidData() {
            let success = false;
             if(this.newEmployeeDetail.id !== "" &&  this.newEmployeeDetail.name !== "" && this.newEmployeeDetail.email !== "") {
                success = true;
            }
            return success;
        },
        onAddEmployee: function(){
            this.cleaMessageStatus();
            if(!this.invalidData()) {
                this.displayErrorMessage = true;
                return;
            }

            this.displaySuccessMessage = true;
            // pass data back to parent
            // The add:employee syntax (as opposed to add-employee)
            // name of the event and data that will be passed to PARENT COMPONENT
            this.$emit('onAdd:employee', this.newEmployeeDetail)
        },
        cleaMessageStatus: function () {
            this.displayErrorMessage = false;
            this.displaySuccessMessage = false;
        }
    }
}
</script>

<style scoped>

#employee-form {
    display: flex;
    flex-direction: column;
    margin-bottom: 2rem;
}

label {
    font-size: 20px;
}
.employee-info {
    width: 50%;
    padding: 12px 20px;
    margin: 8px 0;
    box-sizing: border-box;
    border: 2px solid #ccc
}

button {
  background-color: #4CAF50;
  color: black; 
  border: 2px solid #4CAF50;
    padding: 12px;
    width: 10%;
    margin-top: 10px;
}


  .error-message {
    color: #d33c40;
    font-size: 24px;
    font-weight: bold;
  }

  .success-message {
    color: #32a95d;
    font-size: 24px;
    font-weight: bold;
  }
</style>