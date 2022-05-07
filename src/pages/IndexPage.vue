<template>
  <q-page class="flex flex-center">
    <h2>{{ name }}</h2>
    <q-img :src="url" :ratio="1" width="250px" />

    <div class="row justify-around full-width">
      <q-input filled v-model="id" label="Pesquise por um pokemon" />
      <q-btn color="red" label="Pesquisar" />
    </div>
  </q-page>
</template>

<script>
import api from "src/services/api";

export default {
  name: 'IndexPage',

  data () {
    return {
      name: "",
      url: "",
      id: 1
    };
  },

  beforeMount() {
    api.get('/pokemon/2')
    .then((response) => {
      console.log(response);
      this.name = response.data.name;
      this.url = response.data.sprites.other.dream_world.front_default;
      console.log(this.url);
    })
    .catch(function (error) {
      console.log(error);
    })
    .then(function () {
    });
  }
}
</script>
