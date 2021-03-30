<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      places: [],
    };
  },
  created: function () {
    console.log("in created");
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      console.log("in indexPlaces");
      axios.get("http://localhost:3000/api/places").then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>