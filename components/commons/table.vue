<template>
  <v-card>
    <v-card-title class="heading-3">{{ title }}</v-card-title>
    <v-container>
      <v-data-table :headers="headers" :items="items" item-key="name" class="elevation-1">
        <template v-slot:item.title="{ item }">
          <a @click="showDetail(item.id)">{{ item.title }}</a>
        </template>
        <template v-slot:item.name="{ item }">
          <a @click="showDetail(item.id)">{{ item.name }}</a>
        </template>
      </v-data-table>
    </v-container>
    <v-dialog v-model="showDialog" max-width="290">
      <v-card>
        <v-card-title class="headline">Use Google's location service?</v-card-title>

        <v-card-text>Let Google help apps determine location. This means sending anonymous location data to Google, even when no apps are running.</v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="green darken-1" text @click="showDialog = false">Disagree</v-btn>

          <v-btn color="green darken-1" text @click="dialog = false">Agree</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true
    },
    headers: {
      type: Array,
      required: true
    },
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      colorCard: "#",
      sortBy: "title",
      itemsPerPage: 12,
      totalPage: 0,
      page: 1,
      search: "",
      sortDesc: false,
      itemsPerPageArray: [12, 48, 100, 200],
      showDialog: false
    };
  },
  mounted() {},
  methods: {
    showDetail(id) {
      console.log("emit dialog", id);
      this.$emit("onDetail", id);
    }
  }
};
</script>

<style>
</style>
