<template>
  <div class="header mb-4">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="container">
        <h1 class="navbar-brand m-0">OMDB Search</h1>
        <div class="float-right d-flex">
          <div class="input-group">
            <p class="m-0 pt-2 pr-3" v-show="windowWidth > 768">Search a movie</p>
            <input type="text" class="form-control" v-model="search" />
          </div>
          <button v-on:click="$emit('getmovies', search)" class="btn btn-primary ml-1">search</button>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      windowWidth: null,
      search: "",
    };
  },
  mounted() {
    this.windowWidth = window.innerWidth;
    window.addEventListener("resize", () => {
      this.windowWidth = window.innerWidth;
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.header {
  box-shadow: 0px 4px 5px rgba(0, 0, 0, 0.3);
}

.container{
  padding: 0;
  max-width: 760px;
}
</style>