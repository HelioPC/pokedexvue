<template>
  <q-page class="flex flex-center">

    <div class="column items-center">
      <h2>{{ name }}</h2>
      <q-img :src="url" width="250px" />
    </div>

    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Pesquise por um pokemon" />
      <q-btn color="red" label="Pesquisar" @click="getPokemon" />
    </div>

    <div class="row justify-between absolute full-width">
      <q-icon
        name="far fa-arrow-alt-circle-left"
        class="q-ml-sm cursor-pointer"
        color="primary"
        size="50px"
        @click="getPrevPokemon"
      >
        <q-tooltip> Anterior </q-tooltip>
      </q-icon>
      <q-icon
        name="far fa-arrow-alt-circle-right"
        class="q-mr-sm cursor-pointer"
        color="primary"
        size="50px"
        @click="getNextPokemon"
      >
        <q-tooltip> Próximo </q-tooltip>
      </q-icon>
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
        id: null,
        search: "ditto",
      };
    },

    async beforeMount() {
      await this.getPokemon();
    },

    methods: {
      getPokemon() {
        api.get(`/pokemon/${this.search.toLowerCase()}`)
        .then((response) => {
          this.id = response.data.id;
          this.name = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
        })
        .catch((error) => {
          this.triggerNegative()
        });
      },

      getPrevPokemon() {
        api.get(`/pokemon/${this.id > 1 ? this.id - 1 : 1}`)
        .then((response) => {
          this.id = response.data.id;
          this.name = response.data.name;
          this.search = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
        }
        )
        .catch((error) => {
          this.triggerNegative()
        });
      },

      getNextPokemon() {
        api.get(`/pokemon/${this.id < 944 ? this.id + 1 : 944}`)
        .then((response) => {
          this.id = response.data.id;
          this.name = response.data.name;
          this.search = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
        }
        )
        .catch((error) => {
          this.triggerNegative()
        });
      },

      triggerNegative () {
          this.$q.notify({
            type: 'negative',
            position: 'top',
            message: 'Pokemon não foi encontrado.'
          })
        },
    }
  }
</script>
