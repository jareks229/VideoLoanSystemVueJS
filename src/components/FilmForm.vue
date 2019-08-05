<template>
  <div id="film-form">
    <form @submit.prevent="handleSubmit">
      <label>Nazwa Filmu</label>
  <input
    ref="first"
    type="text"
    :class="{ 'has-error': submitting && invalidName }"
    v-model="film.name"
    @focus="clearStatus"
    @keypress="clearStatus"
  >
  <label>Reżyser</label>
  <input
    type="text"
    :class="{ 'has-error': submitting && invalidDirector }"
    v-model="film.director"
    @focus="clearStatus"
  >

  <label>Gatunek</label>
  <input
    type="text"
    :class="{ 'has-error': submitting && invalidGendre }"
    v-model="film.gendre"
    @focus="clearStatus"
  >
  <label>Ograniczenia wiekowe</label>
  <input
    type="text"
    :class="{ 'has-error': submitting && invalidRestrictions }"
    v-model="film.restrictions"
    @focus="clearStatus"
  >
  <p v-if="error && submitting" class="error-message">
    ❗Please fill out all required fields
  </p>
  <p v-if="success" class="success-message">
    ✅ Film dodany poprawnie
  </p>
  <button>Dodaj Film</button>
    </form>
  </div>
  </template>

<script>
export default {
  name: "film-form",
  data() {
    return {
      error: false,
      submitting: false,
      success: false,
      film: {
        name: '',
        director: '',
        gendre: '',
        restrictions: '',
        avalible: true,
        reserved: false,
        dateAdded: Date.now
      }
    };
  },
  computed: {
    invalidName() {
      return this.film.name === '';
    },

    invalidDirector() {
      return this.film.director === '';
    },

    invalidGendre(){
      return this.film.gendre === '';
    },

    invalidRestrictions(){
      return this.film.restrictions === '';
    }

  },
  methods: {
    handleSubmit() {
      this.clearStatus();
      this.submitting = true;
      
      

      if (this.invalidName || this.invalidDirector || this.invalidGendre || this.invalidRestrictions) {
        this.error = true;
        return;
      }

      this.$emit('add:film', this.film);
      this.$refs.first.focus();
      this.film = {
        name: '',
        director: '',
        gendre: '',
        restrictions: ''
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