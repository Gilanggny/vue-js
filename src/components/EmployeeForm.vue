<template>
  <div id="employee-form">
    <form @submit.prevent="handleEmployee">
      <label>Employee Name</label>
      <input
        type="text"
        v-model="employee.name"
        ref="first"
        @focus="clearStatus"
        @keypress="clearStatus"
        :class="{ 'has-error': submitting && isValidName }"
      />
      <label>Employee Email</label>
      <input
        type="text"
        v-model="employee.email"
        @focus="clearStatus"
        @keypress="clearStatus"
        :class="{ 'has-error': submitting && isValidEmail }"
      />
      <button>Add Employee</button>
      <p v-if="submitting & error" class="error-message">Please fill the valid data</p>
      <p v-if="success" class="success-message">Employee Successfully Added</p>
    </form>
  </div>
</template>

<script>
export default {
  name: "Employee-form",
  data() {
    return {
      submitting: false,
      success: false,
      error: false,
      employee: {
        name: "",
        email: ""
      }
    };
  },
  computed: {
    isValidName() {
      return this.employee.name === "";
    },
    isValidEmail() {
      return this.employee.email === "";
    }
  },
  methods: {
    handleEmployee() {
      this.clearStatus();
      this.submitting = true;

      if (this.isValidName || this.isValidEmail) {
        this.error = true;
        return;
      }
      this.$emit("add:employee", this.employee);
      this.$refs.first.focus();
      this.employee = {
        name: "",
        email: ""
      };
      this.error = false;
      this.submitting = false;
      this.success = true;
    },
    clearStatus() {
      this.success = false;
      this.error = false;
    }
  }
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}

[class*="-message"] {
  font-weight: 500;
}

.error-message {
  color: #d33c40;
}

.success-message {
  color: #32a95d;
}
</style>