<template>
  <q-page class="q-pa-md">
    <q-input v-model="query" label="Buscar películas" outlined class="q-mb-md" />

    <!-- Botón para búsqueda general -->
    <q-btn label="Buscar" color="primary" @click="fetchMovies" />

    <!-- Nuevo botón para búsqueda por nombre -->
    <q-btn label="Búsqueda por Nombre" color="secondary" @click="searchByName" class="q-ml-md" />

    <q-list bordered padding>
      <q-item v-for="movie in movies" :key="movie.id" class="q-mt-md">
        <q-img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`" style="width: 100px;" />
        <q-item-section>
          <q-item-label>{{ movie.title }}</q-item-label>
          <q-item-label>Promedio: {{ movie.vote_average }}</q-item-label>
          <q-item-label>Votos: {{ movie.vote_count }}</q-item-label>
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      query: '',  // El texto de búsqueda que el usuario ingresa
      movies: [],  // Las películas obtenidas de la API
    };
  },
  methods: {
    // Método para obtener películas por nombre o por otros criterios
    async fetchMovies() {
      const API_KEY = "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIxNWFiZmViNTExZGViZGFjYTg3YzE1MDEwNjQ5NmU4ZiIsIm5iZiI6MTczMjE2MzM4OC4zMzQ5MTU2LCJzdWIiOiI2NzNlYjY2YmFiNGQ2ZDBlOGQxYWM5YTIiLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.biDXTTmcvqIlztN104w7RyXLr44S79qyi1sjKHdD7VE";
      const URL = `https://api.themoviedb.org/3/discover/movie?language=es-PE&sort_by=popularity.desc&query=${this.query}`;

      try {
        const response = await axios.get(URL, {
          headers: { Authorization: `Bearer ${API_KEY}` },
        });
        this.movies = response.data.results;
      } catch (error) {
        this.$q.notify({
          type: 'negative',
          message: 'Error al cargar películas',
        });
      }
    },

    // Método para realizar una búsqueda por nombre de película
    async searchByName() {
      const API_KEY = "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIxNWFiZmViNTExZGViZGFjYTg3YzE1MDEwNjQ5NmU4ZiIsIm5iZiI6MTczMjE2MzM4OC4zMzQ5MTU2LCJzdWIiOiI2NzNlYjY2YmFiNGQ2ZDBlOGQxYWM5YTIiLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.biDXTTmcvqIlztN104w7RyXLr44S79qyi1sjKHdD7VE";
      const URL = `https://api.themoviedb.org/3/search/movie?language=es-PE&query=${this.query}`;

      try {
        const response = await axios.get(URL, {
          headers: { Authorization: `Bearer ${API_KEY}` },
        });
        this.movies = response.data.results;
      } catch (error) {
        this.$q.notify({
          type: 'negative',
          message: 'Error al cargar películas por nombre',
        });
      }
    },
  },
};
</script>
