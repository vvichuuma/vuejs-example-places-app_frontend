<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <ul>
      <li v-for="error in errors" class="error">
        
          {{ error }}

      </li> 


    </ul>
    <h2>List_of_Places</h2>
    <h2>Create a new place</h2>
    <div>
        name:<input type="text" v-model="newPlace.name">
        address:<input type="text" v-model="newPlace.address">
        <button v-on:click="createPlace()">Create_place</button>
    </div>
    <div v-for = "place in places">
      
       <h3>{{place.name}}</h3>
        <p>{{place.address}}</p>
        <div>
          Update_place:<input type="text" v-model="updateTitle">
          <button v-on:click="updatePlace(place)" >Update_place</button>
        </div>
      <div>
        <button v-on:click="deletePlace(place)">Delete_place</button>
      </div>

    </div>
  </div>
</template>

<style>
.error {
  color: red;
}
</style>

<script>
var axios = require("axios");
console.log(axios);

export default {
  data: function() {
    return {
      message: "Vishnu a Compter Programmer",
      places: [],
      newPlace: { name: "", address: "" },
      updateTitle: "",
      errors: []
    };
  },
  // DISPLAYING ALL ELEMENTS IN AN ARRAY
  created: function() {
    axios.get("http://localhost:3000/api/places").then(response => {
      console.log(response);
      this.places = response.data;
    });
  },
  //FUNCTIONS:
  methods: {
    //CREATE A PLACE:
    createPlace: function() {
      this.errors = [];
      var params = {
        name: this.newPlace.name,
        address: this.newPlace.address
      };
      axios
        .post("http://localhost:3000/api/places", params)
        .then(response => {
          console.log("the response is", response);
          this.places.push(response.data);
          this.newPlace = { name: "", address: "" };
        })
        .catch(
          function(error) {
            console.log(error.response.data.errors);
            this.errors = error.response.data.errors;
          }.bind(this)
        );
    },

    //UPDATE ACTION:

    updatePlace: function(inputPlace) {
      console.log(this.updateTitle);
      console.log(inputPlace);
      console.log(inputPlace.id);

      var params = {
        name: this.updateTitle
      };

      axios
        .patch("http://localhost:3000/api/places/" + inputPlace.id, params)
        .then(function(response) {
          console.log(response.data);

          inputPlace.name = response.data.name;
        });
    },

    //Delete_Functions:

    deletePlace: function(inputPlace) {
      axios.delete("http://localhost:3000/api/places/" + inputPlace.id).then(
        function(response) {
          console.log(response.data);
          var index = this.places.indexOf(inputPlace);
          this.places.splice(index, 1);
        }.bind(this)
      );
    }
  },
  computed: {}
};
</script>
