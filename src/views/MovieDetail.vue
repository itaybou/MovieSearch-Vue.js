<template>
  <div class="movie-details">
    <div class="title-img">
      <div class="title-years">
        <h2>{{ movie.Title }}</h2>
        <p><span>Released</span>: {{ movie.Released }}</p>
        <p><span>Years</span>: {{ movie.Year }}</p>
        <p><span>Rated</span>: {{ movie.Rated }}</p>
      </div>

      <div class="movie-img">
        <img :src="movie.Poster" alt="Movie Poster" class="featured-img" />
        <div class="type">{{ movie.Type }}</div>
      </div>
    </div>
    <div class="genere">{{ movie.Genre }}</div>
    <div class="details">
      <p class="plot"><span>Plot</span>: {{ movie.Plot }}</p>
      <p class="actors"><span>Actors</span>: {{ movie.Actors }}</p>
    </div>
    <div
      class="ratings"
      v-if="movie.imdbRating !== 'N/A' && movie?.Ratings?.length > 0"
    >
      <p v-if="movie.imdbRating !== 'N/A'">
        <span>IMDB</span>: {{ movie.imdbRating }}
      </p>
      <p v-for="rating in movie.Ratings" :key="rating.Source">
        <span>{{ rating.Source }}</span
        >: {{ rating.Value }}
      </p>
    </div>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import { useRoute } from "vue-router";

export default {
  setup() {
    const movie = ref({});
    const route = useRoute();

    onBeforeMount(() => {
      fetch(
        `http://www.omdbapi.com/?apikey=${process.env.VUE_APP_OMDB_API_KEY}&i=${route.params.id}&plot=full`
      )
        .then((response) => response.json())
        .then((data) => {
          movie.value = data;
          console.log(movie.value);
        });
    });

    return {
      movie,
    };
  },
};
</script>

<style lang="scss">
$phone-br: 640px;

.movie-details {
  padding: 16px;
  h2 {
    color: #fff;
    font-size: 28px;
    font-weight: 600;
    margin-bottom: 16px;
  }

  .featured-img {
    display: block;
    max-width: 450px;
    margin-bottom: 16px;

    @media (max-width: $phone-br) {
      max-width: 200px;
    }
  }

  .title-img {
    display: block;
    justify-content: space-between;

    @media (max-width: $phone-br) {
      display: flex;
    }
  }

  .movie-img {
    position: relative;
    display: block;

    img {
      display: block;
      width: 100%;
      object-fit: inherit;
    }

    .type {
      position: absolute;
      padding: 8px 16px;
      background-color: #42b883;
      color: #fff;
      bottom: 16px;
      left: 0;
      text-transform: capitalize;
    }
  }

  .details {
    display: flex;

    @media (max-width: $phone-br) {
      display: block;
    }
  }

  .genere {
    width: 100%;
    padding: 8px 16px;
    font-size: 18px;
    font-weight: 600;
    border-radius: 10px 10px 0px 0px;
    background-color: #42b883;
    color: #fff;
    bottom: 16px;
    text-transform: capitalize;
  }

  .plot {
    background-color: #496583;
    border-radius: 0px 0px 10px 10px;
    padding: 10px;

    margin-top: 0px;
  }

  .actors {
    padding: 10px;
  }

  .ratings {
    background-color: #3b8070;
    border-radius: 10px;
    padding: 4px 16px 4px 16px;

    @media (min-width: $phone-br) {
      margin-top: 16px;
    }
  }

  p {
    color: #fff;
    font-size: 18px;
    line-height: 1.4;

    @media (max-width: $phone-br) {
      margin-top: 16px;
    }

    @media (min-width: $phone-br) {
      margin-right: 16px;
    }

    span {
      font-weight: bold;
      font-size: 20px;
    }
  }
}
</style>
