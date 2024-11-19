<script setup>
import { ref, onMounted } from 'vue';
import WelcomeItem from '../components/WelcomeItem.vue';
import DocumentationIcon from '../components/icons/IconDocumentation.vue';
import ToolingIcon from '../components/icons/IconTooling.vue';
import EcosystemIcon from '../components/icons/IconEcosystem.vue';
import CommunityIcon from '../components/icons/IconCommunity.vue';
import SupportIcon from '../components/icons/IconSupport.vue';

const selectedMovie = ref('');
const peliculas = ref([]);

// Función asincrónica para obtener películas de la API
async function obtenerPeliculas() {
  try {
    const response = await fetch('https://dummyapi.online/api/movies');
    const data = await response.json();
    
    // Filtra las propiedades necesarias de cada película
    peliculas.value = data.map(pelicula => ({
      movie: pelicula.movie,
      rating: pelicula.rating,
      imdb_url: pelicula.imdb_url
    }));
  } catch (error) {
    console.error('Error al obtener las películas:', error);
  }
}

// Función para seleccionar una película
function seleccionarPelicula(titulo) {
  selectedMovie.value = titulo;
}

// Llamar a obtenerPeliculas cuando el componente se monte
onMounted(() => {
  obtenerPeliculas();
});
</script>

<template>
  <Suspense>
    <div>
      <h1 v-if="selectedMovie">{{ selectedMovie }}</h1>
      <table class="table table-success table-striped" style="width: 80%">
        <thead>
          <tr>
            <th scope="col">Título</th>
            <th scope="col">Calificación</th>
            <th scope="col">IMDB</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="pelicula in peliculas" :key="pelicula.movie">
            <td class="col-titulo" @click="seleccionarPelicula(pelicula.movie)">
              {{ pelicula.movie }}
            </td>
            <td class="col-rating">{{ pelicula.rating }}</td>
            <td class="col-imdb">{{ pelicula.imdb_url }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </Suspense>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
  text-align: center;
  margin: 0 auto; 
}

.col-titulo {
  width: 40%;
  font-size: 1.2rem;
  cursor: pointer;
}

.col-rating {
  color: green;
}

.col-imdb {
  width: 40%;
  font-size: 1rem;
}

th {
  color: white;
  background-color: blue;
  font-weight: bold;
  padding: 10px;
  text-align: center;
}

tbody {
  background-color: lightgray;
  color: black;
}

tbody td {
  padding: 10px;
  text-align: center;
}
</style>