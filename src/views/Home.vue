<template>
  <div>
    <Header v-on:getmovies="getMovies" />
    <div class="movies">
      <div class="d-flex justify-content-center">
        <div v-if="(error && !initialstate)" class="text error">{{error}}</div>
        <div
          v-if="initialstate"
          class="text"
        >Welcome to OMDB Search, search something in the bar above !</div>
      </div>
      <div v-if="!error">
        <div class="container">
          <div class="row no-gutters">
            <div
              v-for="movie in movies"
              :key="movie.imdbID"
              :class="movie.imdbID == openMovieDetails ? 'col-lg-12' : 'col-lg-6'"
            >
              <movieComponent
                v-bind:movie="movie"
                :show="movie.imdbID == openMovieDetails"
                v-on:openMovieDetails="openMovieDetails = $event"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <img :src="require('@/assets/logo.png')" alt="">
  </div>
</template>

<script>
import axios from "axios";

import Header from "../components/layout/Header";
import movieComponent from "../components/movieComponent";


export default {
  name: "Home",
  data() {
    return {
      initialstate: true,
      movies: [],
      error: "",
      search: "",
      openMovieDetails: 0,
    };
  },
  components: {
    movieComponent,
    Header,
  },
  methods: {
    getMovies(search) {
      this.initialstate = false;
      axios
        .get("http://www.omdbapi.com?apikey=61a6fd4a&s=" + search)
        .then((response) => {
          if (response.data.Error) {
            this.error = response.data.Error;
          } else {
            this.error = "";
            this.movies = response.data.Search;
          }
          console.log(response);
        })
        .catch(function (error) {
          // handle error
          console.log(error);
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.text {
  background-color: #f2f5f7;
  text-align: center;
  padding: 20px;
  width: 800px;
}

.error {
  border: dashed 1px red;
  background-color: #fdd0d0;
  color: red;
}
</style>