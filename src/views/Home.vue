<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Add car year here:<input type="text" v-model="newCarYear"></p>
    <p>Add car make here<input type="text" v-model="newCarMake"></p>
    <p>Add car model here<input type="text" v-model="newCarModel"></p>
    <p>Add car color here<input type="text" v-model="newCarColor"></p>
    <p>Add car mileage here<input type="text" v-model="newCarMileage"></p>
    <p>Add car lot here<input type="text" v-model="newCarLot_id"></p>
    <button v-on:click="addCar()">Create Car</button>
    <br>

 <!--     <h3>Search for Car make here:</h3><input type="text" v-model="searchTerm" list="makes">
     <datalist id="makes">
      <option v-for="car in cars">{{car.make}}</option>
    </datalist> -->
     <br>

    <div class="box" v-for="car in cars">
      <ul class="carList">
        <li>id: {{car.id}}</li>
        <li>year: {{car.year}}</li>
        <li>make: {{car.make}}</li>
        <li>model: {{car.model}}</li>
        <li>color: {{car.color}}</li>
        <li>mileage: {{car.mileage}}</li>
        <li>lot id: {{car.lot_id}}</li>
      </ul>
      <button v-on:click="toggleInfo(car)">More Info</button>
      <button v-on:click="destroyCar(car)">Delete Car</button>

      <div v-if="currentCar === car">
        <h3>Update Car</h3>
        <p>Year:</p><input type="text" v-model="car.year">
        <p>Make:</p><input type="text" v-model="car.make">
        <p>Model:</p><input type="text" v-model="car.model">
        <p>Color:</p><input type="text" v-model="car.color">
        <p>Mileage:</p><input type="text" v-model="car.mileage">
        <p>Lot:</p><input type="text" v-model="car.lot_id">
        <br>
        <button v-on:click="updateCar(car)">Update Car</button>
      </div>
    </div>


  </div>
</template>

<style>
.carList {
  list-style: none;
}

.box {
  border: solid;
  border-color: #f9f9f9;
  margin-left: 30%;
  margin-right: 30%;
}

</style>

<script>
import axios from "axios";
import Vue2Filters from 'vue2-filters';
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      cars: [],
      newCarYear:"",
      newCarMake: "",
      newCarModel: "",
      newCarColor: "",
      newCarMileage:"",
      newCarLot_id:"",
      currentCar: {}

    };
  },
  created: function() {
    axios.get(`/api/cars`).then(response => {
      this.cars =  response.data;
      console.log(response);
    });
  },
  methods: {
    addCar: function() {
      console.log("function made");
      var params = {
        year: this.newCarYear,
        make: this.newCarMake,
        model: this.newCarModel,
        color: this.newCarColor,
        mileage: this.newCarMileage,
        lot_id: this.newCarLot_id
      };

      axios.post("/api/cars", params).then(response => {
        console.log(response.data);
        this.cars.push(response.data);
        this.newCarYear = "";
        this.newCarMake = "";
        this.newCarModel = "";
        this.newCarColor = "";
        this.newCarMileage = "";
        this.newCarLot_id = "";
      });
    },

    destroyCar: function(theCar) {
      axios.delete(`/api/cars/${theCar.id}`).then(response => {
        console.log(response.data);
        var index = this.cars.indexOf(theCar);
        console.log(index);
        this.cars.splice(index, 1);
      });
    },

    toggleInfo: function(theCar) {
      if (this.currentCar === theCar) {
        this.currentCar = null;
      } else { 
        this.currentCar = theCar;
      }
      console.log('toggling info...')
      ;
    },

    updateCar: function(theCar) {
      console.log('Updating the car...');
      console.log(theCar);

      var params = {
        year: theCar.year,
        make: theCar.make,
        model: theCar.model,
        color: theCar.color,
        mileage: theCar.mileage,
        lot_id: theCar.lot_id
      };

      axios.patch(`/api/cars/${theCar.id}`, params).then(response => {
        console.log(response.data);
        theCar.year = response.data.year;
        theCar.make = response.data.make;
        theCar.model = response.data.model;
        theCar.color = response.data.color;
        theCar.lot_id = response.data.lot_id;
      });
    }
  }
};
</script>
