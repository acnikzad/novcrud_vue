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
      <button v-on:click="destroyCar(car)">Delete Car</button>
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
    },

    destroyCar: function(theCar) {
      axios.delete(`/api/cars/${theCar.id}`).then(response => {
        console.log(response.data);
        var index = this.cars.indexOf(theCar);
        console.log(index);
        this.cars.splice(index, 1);
      });
    }
  }
};
</script>
