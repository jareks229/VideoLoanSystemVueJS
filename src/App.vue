<template>
  <div id="app" class="small-container">
    <h1>Wypożyczalnia Filmów</h1>
    <div>
    <b-card no-body>
    <b-tabs pills card>
      <b-tab title="Filmy" active> <b-button v-b-toggle.collapse-1 variant="primary">Dodaj Film</b-button>
      <b-button class="ml-1" @click="showOnlyAvalibleFilms">Pokaż tylko dostępne</b-button>

  <b-collapse id="collapse-1" class="mt-2">
    <b-card>
      <film-form @add:film="addFilm"/>
    </b-card>    
  </b-collapse>
    
  <film-table :films="films" @delete:film="deleteFilm" @edit:film="editFilm"/></b-tab>
      <b-tab title="Klienci"> <b-button v-b-toggle.collapse-1 variant="primary">Dodaj Klienta</b-button>
        <b-collapse id="collapse-1" class="mt-2">
      <b-card>
        <customer-form @add:customer="addCustomer"/>
      </b-card>    
    </b-collapse>
        <customer-table :customers="customers" @delete:customer="deleteCustomer" @edit:customer="editCustomer"/></b-tab>
      <b-tab title="Wypożyczone Filmy"></b-tab>
      <b-tab title="Opcje administratora"></b-tab>
    </b-tabs>    
  </b-card>
    </div>
  </div>
</template>

<script>
    import FilmTable from '@/components/FilmTable.vue'
    import FilmForm from '@/components/FilmForm.vue'
    import CustomerTable from '@/components/CustomerTable.vue'
    import CustomerForm from '@/components/CustomerForm.vue'

  export default {
    name: 'app',
    components: {
      FilmTable,
      FilmForm,
      CustomerTable,
      CustomerForm,
    },
    data() {
    return {
      films: [],
    customers: []  
    }
  },

  mounted(){
    this.getFilms()
  },
  
  methods: {
    async getFilms() {
  try {
    const response = await fetch('https://localhost:44385/api/Film')
    const data = await response.json()
    this.films = data
  } catch (error) {
    console.error(error)
  }
},
    
   async addFilm(film) {
    try {
    const response = await fetch('https://localhost:44385/api/Film', {
      method: 'POST',
      body: JSON.stringify(film),
      headers: { 'Content-type': 'application/json; charset=UTF-8' },
    })
    const data = await response.json()
    this.films = [...this.films, data]
  } catch (error) {
    console.error(error)
  }
  },
  async deleteFilm(id) {
    try {
    await fetch(`https://jsonplaceholder.typicode.com/Films/${id}`, {
      method: "DELETE"
    });
    this.films = this.films.filter(film => film.id !== id);
  } catch (error) {
    console.error(error);
  }
  },
  async editFilm(id, updatedFilm) {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/Films/${id}`, {
      method: 'PUT',
      body: JSON.stringify(updatedFilm),
      headers: { 'Content-type': 'application/json; charset=UTF-8' },
    })
    const data = await response.json()
    this.films = this.films.map(film => (film.id === id ? data : film))
  } catch (error) {
    console.error(error)
  }
},

  showOnlyAvalibleFilms() {
    
    this.films = this.films.filter(film => film.avalible == true);
  },

  addCustomer(customer){
    const lastId =
    this.customers.length > 0
      ? this.customers[this.customers.length - 1].id
      : 0;
  const id = lastId + 1;
  const newCustomer = { ...customer, id };
  this.customers = [...this.customers, newCustomer];
  },

  deleteCustomer(id) {
    this.customers = this.customers.filter(
      customer => customer.id !== id
    )
  },
  editCustomer(id, updatedCustomer) {
  this.customers = this.customers.map(customer =>
    customer.id === id ? updatedCustomer : customer
  )
}
  }
  
  }

</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
       
  }

    

  .small-container {
    max-width: 900px;
  }
</style>
