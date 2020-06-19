<template>
  <div>
    <h1>Jokes</h1>
    <SearchJokes @search-text="searchText" />
    <template v-if="jokes.length">
      <Joke
        v-for="joke in jokes"
        :key="joke.id"
        :id="joke.id"
        :joke="joke.joke"
      />
    </template>
    <p v-else>Nothing was found</p>
  </div>
</template>

<script>
import axios from 'axios';
import Joke from '@/components/Joke';
import SearchJokes from '@/components/SearchJokes'

export default {
  components: {
    Joke,
    SearchJokes
  },
  data() {
    return {
      jokes: []
    }
  },
  methods: {
    async searchText(text) {
      const config = {
        headers: {
          'Accept': 'application/json'
        }
      };

      try {
        const response = await axios.get(`https://icanhazdadjoke.com/search?term=${text}`, config);
        this.jokes = response.data.results;
      } catch (error) {
        console.log(error);
      }
    }
  },
  async created() {
    const config = {
      headers: {
        'Accept': 'application/json'
      }
    }

    try {
      const response = await axios.get('https://icanhazdadjoke.com/search', config);
      this.jokes = response.data.results;
    } catch (error) {
      console.log(error);
    }
  },
  head() {
    return {
      title: 'Dad Jokes',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Best place for corny dad jokes'
        }
      ]
    }
  }
}
</script>

<style>

</style>