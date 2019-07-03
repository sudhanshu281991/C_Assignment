<template>
  <div class="employee-form-edit">
    <div id="overlay"></div>
    <form id="employee-form">
      <div class="employee-id">
        <label>Employee ID</label>
        <input
          type="text"
          v-validate="{ required: true,numeric:true }"
          name="id"
          id="id"
          class="form-control"
          v-model="employeeForm.id"
        />
        <span>{{ errors.first('id') }}</span>
      </div>
      <div class="employee-name">
        <label>Employee Name</label>
        <input
          type="text"
          v-validate="{ required: true }"
          name="name"
          id="name"
          class="form-control"
          v-model="employeeForm.name"
        />
        <span>{{ errors.first('name') }}</span>
      </div>
      <div class="employee-avatar">
        <label>Employee Avatar</label>
        <input
          type="text"
          name="avatar"
          id="avatar"
          class="form-control"
          v-model="employeeForm.avatar"
        />
      </div>
      <div class="employee-email">
        <label>Employee Email Address</label>
        <input
          type="text"
          v-validate="{ required: true,email:true }"
          name="email"
          id="email"
          class="form-control"
          v-model="employeeForm.email"
        />
        <span>{{ errors.first('email') }}</span>
      </div>
      <div class="employee-mobile">
        <label>Employee Mobile No</label>
        <input
          type="text"
          v-validate="{ required: true,max:10,numeric:true }"
          name="mobile"
          id="mobile"
          class="form-control"
          v-model="employeeForm.mobile"
        />
        <span>{{ errors.first('mobile') }}</span>
      </div>
      <div class="employee-responsibility">
        <label>Responsibility(skills,role)</label>
        <textarea
          maxlength="200"
          name="responsibility"
          id="responsibility"
          class="form-control"
          v-model="employeeForm.responsibility"
        ></textarea>
      </div>
      <button @click.prevent="submitDetails()">Submit Details</button>
    </form>
  </div>
</template>
<script>
export default {
  name: "EmployeeFormEdit",
  props: ["editedIndex"],
  data() {
    return {
      selectedIndex: -1,
      employeeForm: {}
    };
  },
  created() {
    this.selectedIndex = this.editedIndex;
    this.employeeForm = {
      ...this.$store.getters.getEmployeeData[this.selectedIndex]
    };
  },
  methods: {
    submitDetails() {
      this.$validator.validate().then(valid => {
        if (valid) {
          let intialEmployeeData = this.$store.getters.getEmployeeData;
          intialEmployeeData[this.selectedIndex] = this.employeeForm;
          this.$store.commit("setEmployeeData", intialEmployeeData);
          localStorage.setItem(
            "setEmployeeData",
            JSON.stringify(intialEmployeeData)
          );
          this.$emit("closeOverLay", false);
        }
      });
    }
  }
};
</script>
<style scoped>
#overlay {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0%;
  background: black;
  opacity: 0.6;
}
#employee-form{
    width: 40%;
    background: white;
    z-index: 99999;
    position: absolute;
    left: 33%;
    padding: 30px;

}
</style>


