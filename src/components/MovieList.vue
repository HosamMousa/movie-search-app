<template>
  <div class="preview">
    <div class="container">
      <div v-if="loading" class="loading-container">
        <p class="loading"></p>
      </div>
      <div v-else class="movies">
        <div class="images">
          <div
            v-for="movie in currentMovies"
            :key="movie.id"
            class="images-box"
            data-name="film"
          >
            <img
              :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
              :alt="`${movie.title}`"
              @click="showMovie(movie)"
            />
            <h6 class="rate">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                style="fill: #ffd34c; transform: ; msfilter: "
              >
                <path
                  d="M21.947 9.179a1.001 1.001 0 0 0-.868-.676l-5.701-.453-2.467-5.461a.998.998 0 0 0-1.822-.001L8.622 8.05l-5.701.453a1 1 0 0 0-.619 1.713l4.213 4.107-1.49 6.452a1 1 0 0 0 1.53 1.057L12 18.202l5.445 3.63a1.001 1.001 0 0 0 1.517-1.106l-1.829-6.4 4.536-4.082c.297-.268.406-.686.278-1.065z"
                ></path>
              </svg>
              <p>{{ movie.vote_average.toFixed(1) }}</p>
            </h6>
            <div class="title">
              <h6 title="title">{{ movie.title }}</h6>
            </div>
          </div>
        </div>
        <div class="pagination">
          <button
            v-for="page in totalPages"
            :key="page"
            @click="setPage(page)"
            :class="{ active: currentPage === page }"
          >
            {{ page }}
          </button>
        </div>
      </div>
    </div>
  </div>
  <!-- Passing down four props to a FooterBar child component -->
  <FooterBar
    :highRatedCount="highRatedCount"
    :lowRatedCount="lowRatedCount"
    :totalVotes="totalVotes"
    :actionPercent="actionPercent"
  />
</template>

<script>
import FooterBar from "./FooterBar.vue";
import { ref, onMounted, computed } from "vue";
import axios from "axios";
import Swal from "sweetalert2";

export default {
  name: "MovieList",
  components: {
    FooterBar,
  },
  props: ["findFilms"],
  setup(props) {
    const movies = ref([]);
    const currentPage = ref(1);
    const moviesPerPage = ref(10);
    const loading = ref(true);

    const fetchMovies = async () => {
      try {
        const response = await axios.get(
          "https://api.themoviedb.org/3/trending/movie/week?api_key=5ca27f8c8374461a6c3e6684fc057882"
        );
        movies.value = response.data.results;
      } catch (error) {
        Swal.fire("Error", "Failed to fetch movies", "error");
      } finally {
        loading.value = false;
      }
    };

    onMounted(() => {
      fetchMovies();
    });

    // Filtering the movies and examine if the value from user input (props.findFilms).match(with movies in list)
    const filterFilms = computed(() => {
      if (!props.findFilms) {
        return movies.value;
      }
      const filtering = new RegExp(props.findFilms, "i");
      return movies.value.filter((movie) => {
        return JSON.stringify(movie).match(filtering);
      });
    });

    // Counting the number of the pages
    const currentMovies = computed(() => {
      const startIndex = (currentPage.value - 1) * moviesPerPage.value;
      const endIndex = startIndex + moviesPerPage.value;
      return filterFilms.value.slice(startIndex, endIndex);
    });

    const totalPages = computed(() => {
      return Math.ceil(filterFilms.value.length / moviesPerPage.value);
    });

    const highRatedCount = computed(() => {
      return movies.value.filter((movie) => movie.vote_average > 7).length;
    });
    const lowRatedCount = computed(() => {
      return movies.value.filter((movie) => movie.vote_average < 5).length;
    });

    // Sumtion of vote and examine if is it M or K
    const totalVotes = computed(() => {
      const sum = movies.value.reduce(
        (total, movie) => total + movie.vote_count,
        0
      );
      let count = "";
      let firstTwoNumbers = "";
      if (sum > 1000000) {
        count += "M";
        firstTwoNumbers = Math.floor(sum / 1000000);
      } else if (sum > 1000) {
        count += "K";
        firstTwoNumbers = Math.floor(sum / 1000);
      } else {
        firstTwoNumbers = sum;
      }
      return { sum, count, firstTwoNumbers };
    });

    // Filtering the movies that have a genre_ids = 28 to set type movies to action.
    const actionPercent = computed(() => {
      const actionCount = movies.value.filter((movie) =>
        movie.genre_ids.includes(28)
      ).length;
      return ((actionCount / movies.value.length) * 100).toFixed(0);
    });

    // Using sweetalert2 to show the movie in popup.
    const showMovie = (movie) => {
      Swal.fire({
        title: movie.title,
        html: `<div class="custom">
                            <img src="https://image.tmdb.org/t/p/w500/${
                              movie.poster_path
                            }" alt="${movie.title}" class="movie-img">
                            <div class="movie-info">
                              <p class="overview"><b>Overview:</b> ${
                                movie.overview
                              }</p>
                              <p><b>Release date:</b> ${movie.release_date}</p>
                              <p><b>Genres:</b> ${
                                movie.genre_ids && movie.genre_ids.includes(28)
                                  ? "Action"
                                  : "Drama"
                              }</p>
                              <p><b>Vote average:</b> ${movie.vote_average.toFixed(
                                1
                              )}</p>
                            </div>
                          </div>`,
        showCloseButton: true,
        showConfirmButton: false,
        customClass: {
          title: "text-primary fw-bold",
          htmlContainer: "text-center",
        },
      });
    };

    const setPage = (pageNumber) => {
      currentPage.value = pageNumber;
    };

    return {
      currentPage,
      moviesPerPage,
      loading,
      currentMovies,
      totalPages,
      filterFilms,
      showMovie,
      setPage,
      highRatedCount,
      lowRatedCount,
      totalVotes,
      actionPercent,
    };
  },
};
</script>
