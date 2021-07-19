<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt9140554"
        ><img
          src="https://www.hollywoodreporter.com/wp-content/uploads/2021/07/loki-H-2021.jpg"
          alt="Loki Poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>Loki</h3>
          <p>
            The mercurial villain Loki resumes his role as the God of Mischief
            in a new series that takes place after the events of “Avengers:
            Endgame.”
          </p>
        </div></router-link
      >
    </div>
    <form @submit.prevent="searchMovies()" class="search-box">
      <input
        type="text"
        placeholder="What are you looking for?"
        v-model="search"
      />
      <input type="submit" value="Search" />
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="`/movie/${movie.imdbID}`" class="movie-link"
          ><div class="movie-img">
            <img :src="movie.Poster" alt="Movie Poster" />
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div></router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { ref } from "vue";

export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

    const capitalizeFirstLetter = (str) => {
      return str.charAt(0).toUpperCase() + str.slice(1);
    };

    const searchMovies = () => {
      if (search.value !== "") {
        fetch(
          `http://www.omdbapi.com/?apikey=${
            process.env.VUE_APP_OMDB_API_KEY
          }&s=${capitalizeFirstLetter(search.value)}`
        )
          .then((response) => response.json())
          .then((data) => {
            movies.value = data?.Search;
            search.value = "";
          });
      }
    };

    return {
      search,
      movies,
      searchMovies,
    };
  },
};
</script>

<style lang="scss">
$phone-br: 1024px;
$tablet-br: 1280px;
$tablet-xl-br: 1920px;

.home {
  .feature-card {
    position: relative;

    .featured-img {
      display: block;
      width: 100%;
      height: 300px;
      position: relative;
      object-fit: cover;
      z-index: 0;

      @media (max-width: $phone-br) {
        width: 100%;
      }
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #fff;
        margin-bottom: 16px;
      }

      p {
        color: #fff;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }

        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.3);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 20%;
      flex: 1 1 20%;
      padding: 16px 8px;

      @media (max-width: $tablet-xl-br) {
        max-width: 25%;
        flex: 1 1 33%;
      }

      @media (max-width: $tablet-br) {
        max-width: 33%;
        flex: 1 1 33%;
      }

      @media (max-width: $phone-br) {
        max-width: 50%;
        flex: 1 1 50%;
      }

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

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

        .detail {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color: #aaa;
            font-size: 14px;
          }

          h3 {
            color: #fff;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}
</style>
