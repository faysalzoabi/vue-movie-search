<template>
  <div id="app">
    <header>
      <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <a href="" class="navbar-brand">Movie Search</a>
      </nav>
    </header>

    <main class="container mt-2">
      <form @submit.prevent = "getResults">
        <fieldset class="form-group">
          <label for="searchTerm">Search</label>
          <input v-model="searchTerm" type="text" class="form-control" id="searchTerm" placeholder="Enter your search">
        </fieldset>
        <button type="submit" class="btn btn-primary">GO</button>
      </form>
      
      <section class="row movies-area">
        <div v-if="error" class="alert alert-danger co">
          {{error}}
        </div>
        <Movie 
         class="col-4"
        v-for="movie in results" 
        :key="movie.imdbID" 
        :movie="movie"
        :isInWatchLater="isInWatchLater"
        :addToWatchLater="addToWatchLater"
        />
      </section>

      <section class="mt-2 col-3 row">
        <h3>Watch Later</h3>
        <div class="row" id="watch-later">
          <Movie 
          class="col-12"
          v-for="movie in watchLater" 
          :key="movie.imdbID" 
          :movie="movie"
          :removeWatchLater="removeWatchLater"
            />
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import Movie from '@/components/Movie';
const API_URL = 'http://www.omdbapi.com/?apikey=d54248b1&type=movie&s=';

export default {
  name: 'app',
  components:{
    Movie,
  },
  data(){
    return{
      error:'',
      searchTerm:'',
      results:[],
      watchLater:[],
    }
  },
  methods:{
    async getResults(){
      console.log('hellos')
      const url = `${API_URL}${this.searchTerm}`;
      console.log('url', url)
      const response = await fetch(url);
      const data = await response.json();
      if(data.Error) {
        this.results = [];
        this.error = data.Error;
      } else {
        this.results = data.Search;
        this.error = '';
      }
    },
    addToWatchLater(movie) {
      this.watchLater.push(movie);
    },
    isInWatchLater(movie) {
      return this.watchLater.some(wl => wl.imdbID === movie.imdbID);
    },
    removeWatchLater(movie) {
      const index = this.watchLater.indexOf(movie);
      this.watchLater.splice(index, 1);
    },
  },
};
</script>

<style>
.movies-area{
  justify-content: space-around;
  align-items: flex-start;
}
</style>
