<template>
  <v-container id="home-page">
    <h1 class="display-4" id="title">Inspirational Quote App</h1>
    <v-col col="3">
      <QuoteComponent @addQuote="addBtn"/>
    </v-col>
    <v-col md="3">
    </v-col>

    <div id="button-group">
      <v-btn @click="toggleList">Saved Quotes</v-btn>
      <v-btn @click="toggleInput">Create Quote</v-btn>
    </div>
    <v-expand-transition>
    <div v-show="inputToggle">
      <InputComponent @total="createQuote"/>
    </div>
    </v-expand-transition>



    <v-expand-transition>
    <div v-show="listToggle">
      <QuoteListComponent id="list" @quoteslist="getQuoteList" @removeQuote="removeQuote" :key="listKey"/>
    </div>
    </v-expand-transition>



    <v-snackbar
        color="success"
        max-width="100px"
        :style="{'margin-bottom':100, 'width': 20}" v-model="showSnackbar" :timeout="snackbarTimeout">
      {{ snackbarText }}
      <v-btn style="float: right" text @click="showSnackbar = false"> Close</v-btn>
    </v-snackbar>

  </v-container>


</template>

<script>
import QuoteComponent from "@/components/QuoteComponent";
import InputComponent from "@/components/InputComponent";
import QuoteListComponent from "@/components/QuoteListComponent";

export default {
  name: "MainArea",
  data() {
    return {
      savedQuotes: [],
      newQuote: null,
      inputToggle: '',
      listToggle: '',
      snackbarText: 'Sorry, that quote is already in the list',
      snackbarTimeout: 4000,
      showSnackbar: false,
      listKey: 0
    }
  },
  components: {
    QuoteComponent,
    InputComponent,
    QuoteListComponent
  },
  methods: {
    getQuoteList(val) {
      this.savedQuotes = val
    },
    createQuote(val){
      this.newQuote = val
      this.addQuote()
    },
    //Toggles close both input and list if anyone is open

    toggleInput() {
      if (this.inputToggle) {
        this.inputToggle = ''
      } else {
        this.inputToggle = 'true'
      }
      localStorage.setItem('inputToggle', this.inputToggle)
    },
    toggleList() {
      if (this.listToggle) {
        this.listToggle = ''
      } else {
        this.listToggle = 'true'
      }
      console.log(this.savedQuotes)
      localStorage.setItem('listToggle', this.listToggle)
    },
    addBtn(val) {

      this.newQuote = val;
      this.addQuote()
      console.log(val)
    },
    addQuote() {
      // ensure that the quote is not in the list
      if (this.savedQuotes.some(e=>e.body === this.newQuote.body)) {
        this.snackbarText = 'Sorry, that quote is already in the list'
        this.showSnackbar = true
        return
      }
      this.snackbarText = 'The quote is saved to the list'
      this.showSnackbar = true
      this.savedQuotes.push(this.newQuote);
      this.saveQuotes();
    },
    // parses quotesList to json in order to save to local storage
    saveQuotes() {
      const parsed = JSON.stringify(this.savedQuotes);
      localStorage.setItem('savedQuotes', parsed);
      this.listKey += 1
    },
    removeQuote(val) {
      this.savedQuotes.splice(val, 1)
      this.saveQuotes()
    },
  },
  mounted() {
    if (localStorage.getItem('savedQuotes')) {
      try {
        this.savedQuotes = JSON.parse(localStorage.getItem('savedQuotes'));
      } catch (e) {
        localStorage.removeItem('savedQuotes');
      }
    }
  }
}
</script>

<style scoped>
#home-page {
  width: 90%;
}

#title {
  margin-bottom: 25px;
}

#list {
  margin-top: 25px;
}
button{
  margin-left: 10px;
  margin-right: 10px;
}
</style>