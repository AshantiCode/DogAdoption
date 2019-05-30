

<template>
  <v-container grid-list-md fluid>
    <v-layout wrap>
      <v-flex xs12 sm4 md3 v-for="pet in dogs" :key="pet.breed">
        <app-dog :dog="pet" @addToFavorites></app-dog>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import { Dogs } from "../data/dogs";
import Dog from "../components/Dog.vue";
import axios from "axios";
axios.defaults.baseURL = "https://dog.ceo/api";

export default {
  components: {
    appDog: Dog
  },
  data() {
    return {
      dogs: Dogs
    };
  },
  created() {
    //clearing old images
    this.dogs.forEach(dog => {
      dog.img = "";
    });
    //creates the links for every card by its breed
    const linksArray = this.dogs.map(
      dog => "/breed/" + dog.breed + "/images/random"
    );

    axios
      .all(linksArray.map(link => axios.get(link)))
      .then(
        axios.spread((...res) => {
          this.dogs.forEach((dog, index) => {
            dog.img = res[index].data.message;
          });
        })
      )
      .catch(error => {
        console.log(error);
      });
  }
};
</script>


<style scoped>
p {
  margin: 0;
}

h5 {
  text-transform: capitalize;
}
</style>