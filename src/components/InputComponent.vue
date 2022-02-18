<template>
  <div>
    <v-text-field
        label="Quote"
        v-model="quote"
        clearable
        value="">
    </v-text-field>
    <v-text-field
        label="Author(Optional)"
        v-model="author"
        clearable
        value="">
    </v-text-field>
    <v-text-field
        label="Link(Optional)"
        v-model="link"
        clearable
        value="">
    </v-text-field>
    <v-btn @click="submit">Submit</v-btn>
    <v-snackbar
        color="success"
        max-width="100px"
        :style="{'margin-bottom':100,
                 'width': 20}"
        v-model="showSnackbar"
        :timeout="snackbarTimeout">
      {{ snackbarText }}
      <v-btn
          style="float: right"
          text
          @click="showSnackbar = false">
        Close
      </v-btn>
    </v-snackbar>
  </div>
</template>
<script>


export default {
  name: "InputComponent",
  data() {
    return {
      quote: '',
      author: '',
      link: '',
      savedQuotes: [],
      snackbarText: '',
      snackbarTimeout: 4000,
      showSnackbar: false,

    }
  },
  methods: {
    addQuote() {
      const body = this.author !== '' ? `${this.quote} - ${this.author}` : `${this.quote}`;
      let totalQuote = {'link': this.link, 'body': body}
      if (!this.quote || this.quote === '') {
        this.snackbarText = 'Sorry, your cannot add an empty quote'
        this.showSnackbar = true
        return
      }
      if (this.savedQuotes.includes(body)) {
        this.snackbarText = 'Sorry, that quote is already in the list'
        this.showSnackbar = true
        return
      }

      this.$emit('total', totalQuote);

    },

    submit() {
      this.addQuote()
      this.quote = ''
      this.author = ''
      this.link = ''
    },

  },
  mounted() {
    // this.savedQuotes = JSON.parse(localStorage.getItem('savedQuotes'));
  }
}
</script>

<style scoped>

</style>