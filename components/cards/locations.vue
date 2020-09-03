<template>
  <v-card>
    <v-card-title class="deep-purple--text text--darken-1">{{item.name}}</v-card-title>
    <v-divider/>

    <v-card-text>
      <v-container>
        <v-row>
          <v-col lg="3">
            <v-icon size="110" color="purple darken-1">mdi-google-maps</v-icon>
          </v-col>
          <v-col lg="2">
            <v-row>Name</v-row>
            <v-row>Climate</v-row>
            <v-row>Terrain</v-row>
            <v-row>Surface Water</v-row>
          </v-col>
          <v-col>
            <v-row>: {{ item.name }}</v-row>
            <v-row>: {{item.climate}}</v-row>
            <v-row>: {{item.terrain}}</v-row>
            <v-row>: {{item.surface_water}}</v-row>
          </v-col>
        </v-row>
        <v-row>Residents</v-row>
        <v-row>
          <div v-if="residents.length > 0">
            <v-chip
              class="ma-2"
              color="purple darken-1"
              outlined
              v-for="(resident, i) in residents"
              :key="i"
            >{{ resident }}</v-chip>
          </div>
          <div v-else>
            <v-chip class="ma-2" color="purple darken-1" outlined>-</v-chip>
          </div>
        </v-row>
        <v-row>Films</v-row>
        <v-row>
          <div v-if="films.length > 0">
            <v-chip
              class="ma-2"
              color="purple darken-1"
              outlined
              v-for="(film, i) in films"
              :key="i"
            >{{ film }}</v-chip>
          </div>
          <div v-else>-</div>
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
      residents: [],
      films: []
    };
  },
  watch: {
    urlId: {
      handler: debounce(function() {
        this.loading = true;
        this.residents = [];
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
        this.item = await this.$axios.$get(`/locations/${this.urlId}`);
        this.getResidents();
        this.getFilms();
        this.loading = false;
      } catch (e) {}
    },
    async getResidents() {
      try {
        for (let i = 0; i < this.item.residents.length; i++) {
          let resident = await this.$axios.$get(this.item.residents[i]);
          this.residents.unshift(resident.name);
        }
      } catch (e) {}
    },
    async getFilms() {
      try {
        for (let i = 0; i < this.item.films.length; i++) {
          let film = await this.$axios.$get(this.item.films[i]);
          this.films.unshift(film.title);
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
