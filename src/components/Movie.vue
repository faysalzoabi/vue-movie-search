<template>
  <div class="card col-4">
          <img :src="movie.Poster" :alt="movie.Title" class="card-img-top">
          <div class="card-body">
            <h4 class="card-title">{{movie.Title}}</h4>
            <p class="card-text">{{movie.Year}}</p>
            <button v-if="addToWatchLater && !isInWatchLater(movie)" 
            @click="addToWatchLater(movie)" 
            class="btn btn-success">Watch later</button>

            <button v-if="removeWatchLater" 
            @click="removeWatchLater(movie)" 
            class="btn btn-danger">Remove</button>
          </div>
 </div>
</template>

<script>
export default {
    props:['movie', 'addToWatchLater', 'isInWatchLater', 'removeWatchLater']
}
</script>

<style>

</style>