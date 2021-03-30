<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="place in places">
      {{ place.name }}
      {{ place.address }}
      </div>
      <h3>Add a Place</h3>
      <p>Name: <input type="text" v-model="newPlaceName"></p>
      <p>Address: <input type="text" v-model="newPlaceAddress"></p>
      <button v-on:click="addPlace">ADD</button>
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
      newPlaceName: "",
      newPlaceAddress: "",
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
        this.places = response.data;
      });
    },
    addPlace: function () {
      console.log("adding place....");
      console.log(this.newPlaceName);

      var params = {
        name: this.newPlaceName,
        address: this.newPlaceAddress,
      };

      axios.post("/api/places", params).then((response) => {
        console.log(response.data);
        this.places.push(response.data);
        this.newPlaceName = "";
        this.newPlaceAddress = "";
      });
    },
  },
};
</script>