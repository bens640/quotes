<template>
  <v-card
      class="justify-center"
      style="height: auto; "
      elevation="30">
    <v-container id="quote-container">
      <div>
        <a class="text-decoration-none"
           v-if="link "
           :href="link">
          <h1 class="display-4 quote-text ">{{ quote[0] }}</h1>
          <h2 class="display-3 quote-author" v-if="quote[1]">- {{ quote[1] }}</h2>
        </a>
        <div v-else>
          <h1 class="display-4 quote-text">{{ quote[0] }}</h1>
          <h3 class="display-3 quote-author" v-if="quote[1]">- {{ quote[1] }}</h3>
        </div>
      </div>
      <div id="quote-button-group">
        <v-btn @click="newQuote"> New Quote</v-btn>
        <v-btn @click="addQuote"> Add to List</v-btn>
      </div>
    </v-container>
  </v-card>
</template>

<script>
import {getRandomQuote} from "@/quotes";

export default {
  name: "QuoteComponent",
  data() {
    return {
      quote: 'null',
      link: '',
    }
  },

  components: {},
  methods: {
  // sends current quote to parent component to add to list
    addQuote() {
      const completeQuote = {'link': this.link, 'body': this.quote.join(' - ')}
      this.$emit('addQuote', completeQuote);
    },
    //generates a new quote and splits the string using the '-' delimiter
    newQuote() {
      const q = getRandomQuote()
      this.quote = q.body.split('-')
      this.link = q.link ? q.link : ''
    }
  },
  mounted() {
    this.newQuote()
  }
}
</script>

<style scoped>


#quote-button-group {
  display: flex;
  justify-content: space-around;
  align-self: end;
}

.quote-text {
  padding-bottom: 10px;
  font-family: 'Lexend Deca', sans-serif;
}

.quote-author {
  font-family: 'Lexend Deca', sans-serif;
  font-weight: 300;
  padding-bottom: 20px;

}

a:link, a:visited {
  text-decoration: none;
  color: black;
}
</style>