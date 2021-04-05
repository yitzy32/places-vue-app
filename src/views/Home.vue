<template>
  <div class="home">
    <h1>{{ message }}</h1>
      <h3>Add a Place</h3>
      <small v-for="error in errors">{{ error }} <br></small>
      <p>Name: <input type="text" v-model="newPlaceName"></p>
      <p>Address: <input type="text" v-model="newPlaceAddress"></p>
      <button v-on:click="addPlace">ADD</button>
    <div v-for="place in places">
      {{ place.name }} <br>
      <p><button v-on:click="showPlace(place)">Quick View</button></p>
      </div>
      <dialog id="place-details">
        <form method="dialog">
          <h5>Quick View</h5>
          <p>Edit your inputs</p>
          <p>name: <input type="text" v-model="currentPlace.name"></p>
          <p>The address for this place is: <input type="text" v-model="currentPlace.address"></p>
          <button v-on:click="updatePlace(currentPlace)">Update Place</button>
          <button>Close</button>
          <button v-on:click="destroyPlace(currentPlace)">Destroy Place</button>
        </form>
      </dialog>
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
      currentPlace: {},
      errors: [],
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

      axios
        .post("/api/places", params)
        .then((response) => {
          console.log(response.data);
          this.places.push(response.data);
          this.newPlaceName = "";
          this.newPlaceAddress = "";
          this.errors = [];
        })
        .catch((error) => {
          console.log(error.response.data.error);
          this.errors = error.response.data.error;
        });
    },
    showPlace: function (thePlace) {
      console.log("showing a place....");
      console.log(thePlace);
      this.currentPlace = thePlace;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      console.log("updating location....");

      var params = {
        name: place.name,
        address: place.address,
      };
      axios.patch("/api/places/" + place.id, params).then((response) => {
        console.log("place is updating");
        this.currentPlace = {};
      });
    },
    destroyPlace: function (place) {
      axios.delete("/api/places/" + place.id).then((response) => {
        console.log("successfully destroyed place");
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>