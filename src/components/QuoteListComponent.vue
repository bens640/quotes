<template>
  <v-card
      class="mx-auto "
      max-width="flex">
    <v-toolbar-title>Saved Quotes</v-toolbar-title>
    <ul>
      <v-list subheader>
        <v-divider></v-divider>
        <v-list-item
            v-for="(q, index) in savedQuotes"
            :key="index"
        >
          <v-list-item-content class="list-item ">
            <a class="text-decoration-none"
               v-if="q.link"
               :href="q.link">
              <v-list-item-title class="overflow-auto text-decoration-none" v-text="q.body">
              </v-list-item-title>
            </a>
            <v-list-item-title v-else class="overflow-auto" v-text="q.body">
            </v-list-item-title>
          </v-list-item-content>
          <v-btn class="x-button" @click="removeQuote(index);showSnackbar = true">X</v-btn>
        </v-list-item>
      </v-list>
    </ul>
    <v-snackbar
        color="success"
        max-width="100px"
        :style="{'margin-bottom':60,
                  'width': 20}"
        v-model="showSnackbar"
        :timeout="snackbarTimeout">
      {{ snackbarText }}
      <v-btn style="float:
             right"
             text
             @click="showSnackbar = false">
        Close
      </v-btn>
    </v-snackbar>
    </v-card>
</template>
<script>

export default {
  name: "QuoteListComponent",
  components: {},
  data() {
    return {
      quotes: '',
      savedQuotes: [],
      snackbarText: 'Quote removed from list',
      snackbarTimeout: 4000,
      showSnackbar: false
    }
  },
  methods: {
    //pass data to delete back to parent
    removeQuote(q) {
      this.$emit('removeQuote', q);
    }
  },
  mounted() {
    // Get data from local storage
    if (localStorage.getItem('savedQuotes')) {
      try {
        this.savedQuotes = JSON.parse(localStorage.getItem('savedQuotes'))
      } catch (e) {
        // localStorage.removeItem('savedQuotes');
      }
    }
  }
}
</script>

<style scoped>
.list-item {
  height: 50px;
  word-wrap: break-word;
}

a:link, a:visited {
  text-decoration: none;
  color: black;
}
</style>