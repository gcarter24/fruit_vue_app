<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="fruit in fruits" v-bind:key="fruit.id">
      {{ fruit.name }}
      <hr />
    </div>
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
  },
};
</script>
