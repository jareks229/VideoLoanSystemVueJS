<template>
    <div id="customer-table">
      <p v-if="customers.length < 1" class="empty-table">
    Brak klientów w bazie
  </p>
    <table v-else>
      <thead>
        <tr>
          <th>Imię</th>
          <th>Nazwisko</th>
          <th>Data Urodzenia</th>
          <th>Data Rejestracji</th>
        </tr>
      </thead>
      <tbody> 
          <tr v-for="customer in customers" :key="customer.id">
            <td v-if="editing === customer.id">
                <input type="text" v-model="customer.firstName" />
            </td>
            <td v-else>{{customer.firstName}}</td>
            <td v-if="editing === customer.id">
                <input type="text" v-model="customer.lastName" />
            </td>
            <td v-else>{{customer.lastName}}</td>
            <td v-if="editing === customer.id">
                <input type="text" v-model="customer.dateOfBirth" />
            </td>
            <td v-else>{{customer.dateOfBirth}}</td>
            <td v-if="editing === customer.id">
                <input type="text" v-model="customer.registrationDate" />
            </td>
            <td v-else>{{customer.registrationDate}}</td>
            <td v-if="editing === customer.id">
              <b-dropdown id="dropdown-1" text="Opcje" class="m-md-2">
                <b-dropdown-item @click="editMode(cutomer)">Zapisz</b-dropdown-item>
                <b-dropdown-item @click="editing = null">Anuluj</b-dropdown-item>
              </b-dropdown>                
            </td>
            <td v-else>
              <b-dropdown id="dropdown-1" text="Opcje" class="m-md-2">
                <b-dropdown-item @click="editMode(customer.id)">Edycja</b-dropdown-item>
                <b-dropdown-item @click="$emit('delete:customer', customer.id)">Usuń</b-dropdown-item>
                </b-dropdown>
            </td>
            </tr>
        </tbody>
    </table>
  </div>
</template>

<script>
export default {
    name: 'customer-table',
    props: {
    customers: Array,
  },
  data() {
  return {
    editing: null,       
  }
  
},

methods: {
    editMode(id) {
    this.editing = id    
    },

  cancelEdit(customer) {
  Object.assign(customer, this.catchedCustomer)
  this.editing = null;
    },

  editCustomer(customer) {
    if (customer.firstName === '' || customer.lastName === '' || customer.dateOfBirth === '' || customer.registrationDate === '') return
    this.$emit('edit:customer', customer.id, customer)
    this.editing = null
  }
}
}
</script>

<style>
    button {
    margin: 0 0.5rem 0 0;
  }
</style>



