<template>
  <div class="employee-list" v-if="employeeListData.length  > 0">
    <table>
      <tr>
        <th>Employee Name</th>
        <th>Employee Avatar</th>
        <th>Employee Responsibility</th>
        <th>Actions</th>
      </tr>
      <tr v-for="(employee,index) in employeeListData" :key="index">
        <th>{{employee.name}}</th>
        <th>{{employee.avatar}}</th>
        <th>{{employee.responsibility}}</th>
        <th>
          <button @click="editEmployee(index)">Edit</button>
          <button @click="deleteEmployee(index)">Delete</button>
        </th>
      </tr>
    </table>
    <employee-form-edit v-if="editFormShow" :editedIndex="editedIndex" @closeOverLay="editFormShow=false;editedIndex=-1"></employee-form-edit>
  </div>
</template>
<script>
import EmployeeFormEdit from '../employee-form-edit.component/employee-form-edit.component'
export default {
  name: "EmployeeList",
  components:{
    EmployeeFormEdit
  },
  data() {
    return {
      editFormShow:false,
      editedIndex:0
    };
  },
  computed: {
    employeeListData() {
      return this.$store.getters.getEmployeeData;
    }
  },
  methods:{
    deleteEmployee(index){
       this.employeeListData.splice(index,1);
       this.$store.commit("setEmployeeData", this.employeeListData);
       localStorage.setItem('setEmployeeData',JSON.stringify(this.employeeListData))
    },
    editEmployee(index){
       this.editedIndex = index;
       this.editFormShow = true;
    }
  }
};
</script>