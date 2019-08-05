<template>
  <div id="customer-form">
    <form @submit.prevent="handleSubmit">
      <label>Imię</label>
  <input
    ref="first"
    type="text"
    :class="{ 'has-error': submitting && invalidFirstName }"
    v-model="customer.firstName"
    @focus="clearStatus"
    @keypress="clearStatus"
  >
  <label>Nazwisko</label>
  <input
    type="text"
    :class="{ 'has-error': submitting && invalidLastName }"
    v-model="customer.lastName"
    @focus="clearStatus"
  >
  <label>Data Urodzenia</label>
  <input
    type="date"
    :class="{ 'has-error': submitting && invalidDate }"
    v-model="customer.dateOfBirth"
    @focus="clearStatus"
  >
   <p v-if="error && submitting" class="error-message">
    ❗Proszę wypełnić wszystkie pola !
  </p>
  <p v-if="success" class="success-message">
    ✅ Klient dodany poprawnie
  </p>
  <button>Dodaj Klienta</button>
    </form>
  </div>
  </template>

<script>
export default {
  name: "customer-form",
  data() {
    return {
      error: false,
      submitting: false,
      success: false,
      customer: {
        firstName: '',
        lastName: '',
        dateOfBirth: '',
        registrationDate: Date.now
      }
    };
  },
  computed: {
    invalidFirstName() {
      return this.customer.firstName === '';
    },

    invalidLastName() {
      return this.customer.lastName === '';
    },

    invalidDate(){
      return this.customer.dateOfBirth === '';
    }    

  },
  methods: {
    handleSubmit() {
      this.clearStatus();
      this.submitting = true;
      
      

      if (this.firstName || this.lastName || this.dateOfBirth) {
        this.error = true;
        return;
      }

      this.$emit('add:customer', this.customer);
      this.$refs.first.focus();
      this.customer = {
        firstName: '',
        lastName: '',
        dateOfBirth: '',
        registrationDate: ''
      };
      
      this.clearStatus();
      this.submitting = false
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

  [class*='-message'] {
    font-weight: 500;
  }

  .error-message {
    color: #d33c40;
  }

  .success-message {
    color: #32a95d;
  }
</style>