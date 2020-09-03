<template>
  <v-card>
    <v-card-title class="deep-purple--text text--darken-1">{{item.name}}</v-card-title>
    <v-divider/>
    <v-card-text>
      <v-container>
        <v-row>
          <v-col lg="3">
            <v-icon size="110" color="purple darken-1">mdi-account-outline</v-icon>
          </v-col>
          <v-col lg="2">
            <v-row>Name</v-row>
            <v-row>Gender</v-row>
            <v-row>Age</v-row>
            <v-row>Eye Color</v-row>
            <v-row>Hair Color</v-row>
            <v-row>Films</v-row>
          </v-col>
          <v-col>
            <v-row>: {{ item.name }}</v-row>
            <v-row>: {{item.gender}}</v-row>
            <v-row>: {{item.age}}</v-row>
            <v-row>: {{item.eye_color}}</v-row>
            <v-row>: {{item.hair_color}}</v-row>
            <v-row>: {{films.title}}</v-row>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn color="blue darken-1" text @click="onClose()">Close</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import debounce from "lodash/debounce";

export default {
  props: {
    urlId: {
      type: String,
      required: true,
      default: ""
    }
  },
  data() {
    return {
      item: {},
      loading: true,
      films: []
    };
  },
  watch: {
    urlId: {
      handler: debounce(function() {
        this.loading = true;
        this.films = [];
        this.getItem();
        this.loading = false;
      })
    }
  },
  mounted() {
    this.getItem();
  },
  methods: {
    async getItem() {
      try {
        this.item = await this.$axios.$get(`/people/${this.urlId}`);
        this.getFilms();
        this.loading = false;
      } catch (e) {}
    },
    async getFilms() {
      try {
        for (let i = 0; i < this.item.films.length; i++) {
          this.films = await this.$axios.$get(this.item.films[i]);
        }
      } catch (e) {}
    },
    onClose() {
      this.$emit("onClose");
    }
  }
};
</script>

<style>
</style>
