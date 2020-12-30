<template>
  <div class="movie mb-3 mx-2">
    <div class="container">
      <div class="row">
        <div class="p-0" :class="show ? 'col-lg-2 col-3' : 'col-lg-4 col-3'">
          <img :src="movie.Poster" alt class="poster" />
        </div>
        <div :class="show ? 'col-lg-10 col-9' : 'col-lg-8 col-9'" class="movietext d-flex flex-column p-0">
          <div class="pl-3 pt-3">{{movie.Title}}</div>
          <div v-if="show" class="p-3">
            <p class="detailTitle">Plot</p>
            <p>{{data.Plot}}</p>
            <p class="detailTitle">Actors</p>
            <p>{{data.Actors}}</p>
            <p class="detailTitle">Ratings</p>
            <p v-for="(rating,i) in data.Ratings" :key="i" class="m-0">- {{rating.Source}} {{rating.Value}}</p>
          </div>
          <div class="p-3 bottom w-100">
            <div class="col-4 p-0 fadetext d-flex align-items-center">
              <p class="m-0">{{movie.Year}}</p>
            </div>
            <div class=".col-4 ml-auto" v-if="!show">
              <button class="btn btn-primary" @click="getMovieDetails">details</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "movieComponent",
  props: ["movie", "show"],
  data() {
    return {
      data: null,
    };
  },
  methods: {
    getMovieDetails() {
      axios
        .get("http://www.omdbapi.com?apikey=61a6fd4a&i=" + this.movie.imdbID)
        .then(async (response) => {
          this.$emit("openMovieDetails", this.movie.imdbID);
          this.data = response.data;
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
.movie {
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.25);
  background-color: #F2F5F7;
}

.movietext{
  background-color: white;
}

.movie .poster {
  width: 100%;
  background-color: rgba(0,0,0,0);
}

.fadetext {
  color: rgba(0, 0, 0, 0.3);
}

.bottom {
  align-self: flex-end;
  margin-top: auto;
  border-top: 1px rgba(0, 0, 0, 0.2) solid;
  display: flex;
  align-content: center;
}

.detailTitle{
  margin: 0;
  color: rgba(0, 0, 0, 0.5);
  font-size: small;
}
</style>