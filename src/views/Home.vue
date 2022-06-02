<template>
  <main class="quotes">
    <h1>{{ character }}</h1>

    <h3 class="quotes__house" v-if="house != undefined">{{ house }}</h3>

    <h2>{{ quote }}</h2>

    <button class="quotes__button" @click="fetchQuote">QUOTE!</button>
  </main>
</template>

<script>
export default {
  data() {
    return {
      house: "House",
      character: "Character",
      quote: "Quote",
    };
  },

  methods: {
    async fetchQuote() {
      const url = "https://api.gameofthronesquotes.xyz/v1/random";
      const response = await fetch(url);
      try {
        this.handleQuoteFetch(response);
      } catch (error) {
        this.error = error.message;
        console.log(error);
      }
    },

    async handleQuoteFetch(response) {
      if (response.status >= 200 && response.status < 300) {
        const results = await response.json();
        this.house = results.character.house.name;
        this.character = results.character.name;
        this.quote = results.sentence;
      } else {
        if (response.status === 404) {
          throw new Error("Url ikke funnet.");
        }
        if (response.status === 401) {
          throw new Error("Ikke authorisert.");
        }
        if (response.status > 500) {
          throw new Error("Server error.");
        }
        throw new Error("Her gikk noe galt.");
      }
    },
  },
};
</script>

<style>
.quotes {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.quotes__house {
  padding: 1rem 0 3rem 0;
}

.quotes__button {
  margin-top: 3rem;
}
</style>