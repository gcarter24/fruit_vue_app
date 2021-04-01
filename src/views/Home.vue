<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>
      Name:
      <input type="text" v-model="newFruitName" placeholder="Enter Name" />
    </p>

    <p>
      Tastiness:
      <input type="text" v-model="newFruitTastiness" placeholder="Enter Tastiness 1-10" />
    </p>

    <p>
      Color:
      <input type="text" v-model="newFruitColor" placeholder="Enter Color" />
    </p>
    <p><button v-on:click="createFruit">Add Fruit</button></p>
    <h2>List of Fruits</h2>
    <div v-for="fruit in fruits" v-bind:key="fruit.id">
      {{ fruit.name }}
      <p>
        <button v-on:click="showFruit(fruit)">More Info</button>
      </p>
      <hr />
    </div>
    <dialog id="fruit-details">
      <form method="dialog">
        <p>
          Name:
          <input type="text" v-model="currentFruit.name" />
        </p>
        <p>
          Tastiness:
          <input type="text" v-model="currentFruit.tastiness" />
        </p>
        <p>
          Color:
          <input type="text" v-model="currentFruit.color" />
        </p>
        <button v-on:click="updateFruit(currentFruit)">Save Changes</button>
        <button>Close</button>
        <button v-on:click="destroyFruit(currentFruit)">Remove</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Fruit App!",
      fruits: [],
      newFruitName: "",
      newFruitTastiness: "",
      newFruitColor: "",
      currentFruit: {},
    };
  },
  created: function () {
    this.indexFruits();
  },
  methods: {
    indexFruits: function () {
      console.log("index");
      axios.get("http://localhost:3000/api/fruits").then((response) => {
        console.log(response.data);
        this.fruits = response.data;
      });
    },
    createFruit: function () {
      console.log("create");
      var params = {
        name: this.newFruitName,
        tastiness: this.newFruitTastiness,
        color: this.newFruitColor,
      };
      axios.post("http://localhost:3000/api/fruits", params).then((response) => {
        console.log(response.data);
        this.fruits.push(response.data);
        this.newFruitName = "";
        this.newFruitTastiness = "";
        this.newFruitColor = "";
      });
    },
    showFruit: function (theFruit) {
      console.log(theFruit);
      console.log("show");
      this.currentFruit = theFruit;
      document.querySelector("#fruit-details").showModal();
    },
    updateFruit: function (theFruit) {
      console.log(theFruit);
      var params = {
        name: this.currentFruit.name,
        tastiness: this.currentFruit.tastiness,
        color: this.currentFruit.color,
      };
      axios.patch("/api/fruits/" + theFruit.id, params).then((response) => {
        console.log(response.data);
        theFruit = response.data;
      });
    },
    destroyFruit: function (theFruit) {
      console.log(theFruit);
      axios.delete("/api/fruits/" + theFruit.id).then((response) => {
        console.log(response.data);
        var index = this.fruits.indexOf(theFruit);
        this.fruits.splice(index, 1);
      });
    },
  },
};
</script>
