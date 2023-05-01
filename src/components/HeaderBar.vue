<template>
  <header>
    <div class="overly"></div>
    <div class="container">
      <div class="content">
        <h1>Trending Movies</h1>
        <p>Search and explore any movies within a second</p>
        <div class="search-box">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            style="fill: rgba(141, 141, 141, 1); transform: ; msfilter: "
          >
            <path
              d="M10 18a7.952 7.952 0 0 0 4.897-1.688l4.396 4.396 1.414-1.414-4.396-4.396A7.952 7.952 0 0 0 18 10c0-4.411-3.589-8-8-8s-8 3.589-8 8 3.589 8 8 8zm0-14c3.309 0 6 2.691 6 6s-2.691 6-6 6-6-2.691-6-6 2.691-6 6-6z"
            ></path>
          </svg>
          <input
            type="text"
            id="search"
            placeholder="Search a movie"
            autofocus
            v-model="inputValue"
          />
        </div>
      </div>
    </div>
  </header>
  <MovieList :findFilms="inputValue" />
</template>

<script>
import MovieList from "./MovieList.vue";

export default {
  name: "HeaderBar",
  components: {
    MovieList,
  },
  data() {
    return {
      inputValue: "",
    };
  },
};
</script>

<style scoped>
.content p {
  margin-top: 8px;
  font-size: 24px;
}

.container .search-box {
  height: 55px;
  margin: 45px 0;
  position: relative;
}
.search-box input {
  width: 100%;
  height: 100%;
  outline: none;
  border: none;
  font-size: 17px;
  padding-left: 55px;
  background: #fff;
  border-radius: 5px;
}

.search-box svg {
  position: absolute;
  left: 20px;
  top: 50%;
  cursor: default;
  color: #8d8d8d;
  font-size: 22px;
  transform: translateY(-50%);
}
</style>
