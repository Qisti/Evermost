<template>
  <v-card>
    <v-card-title class="deep-purple--text text--darken-1">{{item.name}}</v-card-title>
    <v-divider/>

    <v-card-text>
      <v-container>
        <v-row>
          <v-col lg="3">
            <v-icon size="110" color="purple darken-1">mdi-steering</v-icon>
          </v-col>
          <v-col lg="2">
            <v-row>Name</v-row>
            <v-row>Vehicle Class</v-row>
            <v-row>Length</v-row>
            <v-row>Pilot</v-row>
            <v-row>Films</v-row>
          </v-col>
          <v-col>
            <v-row>: {{ item.name }}</v-row>
            <v-row>: {{item.vehicle_class}}</v-row>
            <v-row>: {{item.length}}</v-row>
            <v-row>: {{pilot.name}}</v-row>
            <v-row>: {{films.title}}</v-row>
          </v-col>
        </v-row>
        <v-row>Description :</v-row>
        <v-row class="black--text">{{item.description}}</v-row>
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
      pilot: [],
      films: []
    };
  },
  watch: {
    urlId: {
      handler: debounce(function() {
        this.loading = true;
        this.getItem();
        this.loading = false;
      })
    },
    films: {
      handler: debounce(function() {
        this.getFilms();
      })
    },
    pilot: {
      handler: debounce(function() {
        this.getPilot();
      })
    }
  },
  mounted() {
    this.getItem();
  },
  methods: {
    async getItem() {
      try {
        this.item = await this.$axios.$get(`/vehicles/${this.urlId}`);
        this.getFilms();
        this.getPilot();
        this.loading = false;
      } catch (e) {}
    },
    async getFilms() {
      try {
        this.films = await this.$axios.$get(this.item.films);
      } catch (e) {}
    },
    async getPilot() {
      try {
        this.pilot = await this.$axios.$get(this.item.pilot);
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
