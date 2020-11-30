<template>
  <main class="p-10">
    <form @submit.prevent="search">
      <div class="flex justify-center m-auto">
        <input v-model="q" type="text" placeholder="search" class="rounded-md lg:w-2/12 bg-gray-100 pl-4">
        <button type="submit" class="bg-blue-300 p-2 rounded-md ml-4 text-white">Search</button>
      </div>
    </form>
    <div class="flex flex-wrap justify-around">
      <div v-for="img of imgs" :key="img.title" class="w-64 bg-purple-300 p-1 rounded-md mt-4">
        <img :src="img.poster" alt="">
        <div class="text-center text-xl font-extrabold">{{img.title}}</div>
        <div class="text-center">{{img.date}}</div>
      </div>
    </div>
  </main>
</template>

<script>
  const url = "https://api.themoviedb.org/3";
  const baseUrl = url + '/discover/movie?sort_by=popularity.desc&api_key=04c35731a5ee918f014970082a0088b1&page=1';
  const searchURL = url + '/search/movie?&api_key=04c35731a5ee918f014970082a0088b1&query=';
  const bg = 'https://image.tmdb.org/t/p/w1280';
  export default {
    data: () => ({
      imgs: [],
      q: ""
    }),
    methods: {
      getFilms: async function () {
        const response = await fetch(baseUrl);
        this.imgs = this.parseImgResponse(await response.json())
      },

      parseImgResponse(movies) {
        return movies.results.reduce((acc, movie) => {
          if (!movie.poster_path) {
            return acc
          }
          acc.push({
            poster: `${bg}${movie.poster_path}`,
            date: movie.release_date,
            title: movie.title
          });
          return acc;
        }, []);
      },

      search: async function () {
        if (this.q === "") {
          return;
        }
        const response = await fetch(`${searchURL}${this.q}`);
        this.imgs = this.parseImgResponse(await response.json());
      }
    },
    mounted() {
      this.getFilms()
    }
  }
</script>
