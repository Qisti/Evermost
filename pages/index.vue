<template>
  <div>
    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      class="elevation-1"
      app
      color="deep-purple lighten-1"
      dark
    >
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <div></div>
          <v-img src="/ghibli-invert.png" alt="Vuetify" max-width="75px" dark></v-img>
          <v-btn class="mx-2" text @click="$vuetify.goTo('#films')">Films</v-btn>
          <v-btn class="mx-2" text @click="$vuetify.goTo('#people')">People</v-btn>
          <v-btn class="mx-2" text @click="$vuetify.goTo('#vehicles')">Vehicles</v-btn>
          <v-btn class="mx-2" text @click="$vuetify.goTo('#species')">Species</v-btn>
          <v-btn class="mx-2" text @click="$vuetify.goTo('#locations')">Locations</v-btn>
        </v-row>
      </v-container>
    </v-app-bar>
    <v-container fluid>
      <v-row align="center" justify="center" class="pt-4">
        <v-col cols="12" sm="10" md="10" lg="8" class="pt-4">
          <div id="films" class="pt-4 my-4">
            <card-table
              :items="films"
              :headers="headerFilms"
              :title="titleFilms"
              @onDetail="onDetailFilms"
              @onClose="detailFilms = false"
            ></card-table>
          </div>
          <div id="people" class="my-4">
            <card-table
              :items="people"
              :headers="headerPeople"
              :title="titlePeople"
              @onDetail="onDetailPeople"
              @onClose="detailPeople = false"
            ></card-table>
          </div>
          <div id="vehicles" class="my-4">
            <card-table
              :items="vehicles"
              :headers="headerVehicles"
              :title="titleVehicles"
              @onDetail="onDetailVehicles"
              @onClose="detailVehicles = false"
            ></card-table>
          </div>
          <div id="species" class="my-4">
            <card-table
              :items="species"
              :headers="headerSpecies"
              :title="titleSpecies"
              @onDetail="onDetailSpecies"
              @onClose="detailSpecies = false"
            ></card-table>
          </div>
          <div id="locations" class="my-4">
            <card-table
              :items="locations"
              :headers="headerLocations"
              :title="titleLocations"
              @onDetail="onDetailLocation"
              @onClose="detailLocations = false"
            ></card-table>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <div>
      <v-dialog v-model="detailFilms" class="mx-auto" max-width="600">
        <card-film :urlId="id" @onClose="detailFilms = false"/>
      </v-dialog>
      <v-dialog v-model="detailPeople" class="mx-auto" max-width="600">
        <card-people :urlId="id" @onClose="detailPeople = false"/>
      </v-dialog>
      <v-dialog v-model="detailVehicles" class="mx-auto" max-width="600">
        <card-vehicles :urlId="id" @onClose="detailVehicles = false"/>
      </v-dialog>
      <v-dialog v-model="detailSpecies" class="mx-auto" max-width="600">
        <card-species :urlId="id" @onClose="detailSpecies = false"/>
      </v-dialog>
      <v-dialog v-model="detailLocations" class="mx-auto" max-width="600">
        <card-location :urlId="id" @onClose="detailLocations = false"/>
      </v-dialog>
    </div>
  </div>
</template>

<script>
import Table from "~/components/commons/table.vue";
import CardFilm from "~/components/cards/films.vue";
import CardPeople from "~/components/cards/people.vue";
import CardVehicles from "~/components/cards/vehicles.vue";
import CardSpecies from "~/components/cards/species.vue";
import CardLocations from "~/components/cards/locations.vue";
export default {
  components: {
    "card-table": Table,
    "card-film": CardFilm,
    "card-people": CardPeople,
    "card-vehicles": CardVehicles,
    "card-species": CardSpecies,
    "card-location": CardLocations
  },
  data() {
    return {
      items: ["Films", "People"],
      films: [],
      headerFilms: [
        { text: "Title", value: "title" },
        { text: "Producer", value: "producer" },
        { text: "Rating Score", value: "rt_score" }
      ],
      titleFilms: "Films",
      urlFilms: "/films",
      detailFilms: false,
      people: [],
      headerPeople: [
        { text: "Name", value: "name" },
        { text: "Gender", value: "gender" }
      ],
      titlePeople: "People",
      urlPeople: "/people",
      detailPeople: false,
      vehicles: [],
      headerVehicles: [
        { text: "Name", value: "name" },
        { text: "Class", value: "vehicles_class" }
      ],
      titleVehicles: "Vehicles",
      urlVehicles: "/vehicles",
      detailVehicles: false,
      species: [],
      headerSpecies: [
        { text: "Name", value: "name" },
        { text: "Classification", value: "classification" }
      ],
      titleSpecies: "Species",
      urlSpecies: "/species",
      detailSpecies: false,
      locations: [],
      headerLocations: [
        { text: "Name", value: "name" },
        { text: "Climate", value: "climate" },
        { text: "Terrain", value: "terrain" }
      ],
      titleLocations: "Locations",
      urlLocations: "/locations",
      detailLocations: false,
      id: ""
    };
  },
  mounted() {
    this.getFilms();
    this.getPeople();
    this.getVehicles();
    this.getSpecies();
    this.getLocations();
  },
  methods: {
    async getFilms() {
      try {
        this.films = await this.$axios.$get(this.urlFilms);
      } catch (e) {}
    },
    async getPeople() {
      try {
        this.people = await this.$axios.$get(this.urlPeople);
      } catch (e) {}
    },
    async getVehicles() {
      try {
        this.vehicles = await this.$axios.$get(this.urlVehicles);
      } catch (e) {}
    },
    async getSpecies() {
      try {
        this.species = await this.$axios.$get(this.urlSpecies);
      } catch (e) {}
    },
    async getLocations() {
      try {
        this.locations = await this.$axios.$get(this.urlLocations);
      } catch (e) {}
    },
    onDetailFilms(id) {
      this.id = id;
      this.detailFilms = true;
    },
    onDetailPeople(id) {
      this.id = id;
      this.detailPeople = true;
    },
    onDetailVehicles(id) {
      this.id = id;
      this.detailVehicles = true;
    },
    onDetailSpecies(id) {
      this.id = id;
      this.detailSpecies = true;
    },
    onDetailLocation(id) {
      this.id = id;
      this.detailLocations = true;
    }
  }
};
</script>

<style>
</style>
