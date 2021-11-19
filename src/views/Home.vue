<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <h1>New Place</h1>
    Name:
    <input type="text" v-model="newNameParams.name" />
    Address:
    <input type="text" v-model="newAddressParams.address" />
    <dialog id="place-details">
      <form method="dialog">
        <h1>Place Info:</h1>
        <p>
          Name:
          <input type="text" v-model="currentPlace.name" />
        </p>
        <p>
          Address:
          <input type="text" v-model="currentPlace.address" />
        </p>
        <button v-on:click="updatePlace(currentPlace)">Update Place</button>
        <button v-on:click="destroyPlace(currentPlace)">Delete</button>
        <button>close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
img {
  width: 250px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Places that have Beer!!",
      places: [],
      newPlaceParams: {},
      currentPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("/places").then((response) => {
        this.places = response.data;
        console.log("All places", this.places);
      });
    },
    createPlace: function () {
      console.log("Showing a new place!");
      axios
        .post("/places", this.newPlaceParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.places.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      axios.patch("/places/" + place.id, place).then((response) => {
        console.log("Success", response.data);
      });
    },
    destroyPlace: function (place) {
      axios.delete("/places/" + place.id).then((response) => {
        console.log("Success!", response.data);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>
