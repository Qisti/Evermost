<template>
  <v-card class="mx-auto" max-width="600" :loading="loading">
    <v-card-title class="deep-purple--text text--darken-1">{{item.title}}</v-card-title>

    <v-card-text>
      <v-row align="center" class="mx-0">
        <v-rating :value="rtScore" color="amber" dense half-increments readonly size="14"></v-rating>

        <div class="grey--text ml-4">{{rtScore}}</div>
      </v-row>

      <div class="mt-4 subtitle-1">Director: {{ item.director }}</div>
      <div class="subtitle-1">Producer: {{ item.producer }}</div>
    </v-card-text>
    <v-divider class="mx-4"></v-divider>
    <div class="px-4 mt-4 mx-2 text-justify">{{item.description}}</div>

    <v-divider class="mt-4"/>
    <v-card-title class="deep-purple--text text--darken-1">Review</v-card-title>

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
      required: true
    }
  },
  data() {
    return {
      item: {},
      rtScore: 0,
      showPeople: false,
      showVehicles: false,
      showSpecies: false,
      loading: true
    };
  },
  watch: {
    urlId: {
      handler: debounce(function() {
        this.loading = true;
        this.getItem();
        this.loading = false;
      }, 500),
      deep: true
    }
  },
  mounted() {
    this.getItem();
  },
  methods: {
    async getItem() {
      try {
        this.item = await this.$axios.$get(`/films/${this.urlId}`);
        this.rtScore = parseFloat(this.item.rt_score / 20);
        this.loading = false;
      } catch (e) {
        console.log(e);
      }
    },
    onClose() {
      this.$emit("onClose");
    }
  }
};
</script>

<style>
</style>
