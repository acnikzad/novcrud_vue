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
    <ul class="carList" v-for="car in cars">
      <li>id: {{car.id}}</li>
      <li>year: {{car.year}}</li>
      <li>make: {{car.make}}</li>
      <li>model: {{car.model}}</li>
      <li>color: {{car.color}}</li>
      <li>mileage: {{car.mileage}}</li>
      <li>lot_id: {{car.lot_id}}</li>
      <br>
    </ul>
  </div>
</template>

<style>
.carList {
  list-style: none;
}
</style>

<script>
import axios from "axios";
export default {
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
      });
    }
  }
};
</script>
