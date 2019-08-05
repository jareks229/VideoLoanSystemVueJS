<template>
  <div id="film-table">
      <p v-if="films.length < 1" class="empty-table">
    Brak filmów w bazie
  </p>
    <table v-else>
      <thead>
        <tr>
          <th>Tytuł filmu</th>
          <th>Reżyser</th>
          <th>Gatunek</th>
          <th>Od Lat ?</th>
          <th>Dostępna ?</th>
          <th>Opcje</th>
        </tr>
      </thead>
      <tbody> 
          <tr v-for="film in films" :key="film.id">
            <td v-if="editing === film.id">
                <input type="text" v-model="film.name" />
            </td>
            <td v-else>{{film.name}}</td>
            <td v-if="editing === film.id">
                <input type="text" v-model="film.director" />
            </td>
            <td v-else>{{film.director}}</td>
            <td v-if="editing === film.id">
                <input type="text" v-model="film.gendre" />
            </td>
            <td v-else>{{film.gendre}}</td>
            <td v-if="editing === film.id">
                <input type="text" v-model="film.restrictions" />
            </td>
            <td v-else>{{film.restrictions}}</td>
            <td v-if="film.avalible == true">TAK</td>
            <td v-else>NIE</td>
            <td v-if="editing === film.id">
              <b-dropdown id="dropdown-1" text="Opcje" class="m-md-2">
                <b-dropdown-item @click="editMode(film)">Zapisz</b-dropdown-item>
                <b-dropdown-item @click="editing = null">Anuluj</b-dropdown-item>
              </b-dropdown>                
            </td>
            <td v-else>
              <b-dropdown id="dropdown-1" text="Opcje" class="m-md-2">
                <b-dropdown-item @click="editMode(film.id)">Edycja</b-dropdown-item>
                <b-dropdown-item @click="$emit('delete:film', film.id)">Usuń</b-dropdown-item>
                <b-dropdown-item @click="loanFilm(film)">Wypożycz</b-dropdown-item>
                <b-dropdown-item @click="returnFilm(film)">Zwróć</b-dropdown-item>
              </b-dropdown>
            </td>
            </tr>
        </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'film-table',
    props: {
    films: Array,
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

  cancelEdit(film) {
  Object.assign(film, this.cachedFilm)
  this.editing = null;
    },

  editFilm(film) {
    if (film.name === '' || film.director === '' || film.gendre === '' || film.restrictions === '') return
    this.$emit('edit:film', film.id, film)
    this.editing = null
  },

  loanFilm(film){
    film.avalible = false;
  },

  returnFilm(film) {
    if(film.avalible == true)
    alert("Film nie jest wypożyczony !");
    else(film.avalible == false)
    film.avalible = true;
            
    
    
  }
}
  }
</script>

<style scoped>
 button {
    margin: 0 0.5rem 0 0;
  }
</style>