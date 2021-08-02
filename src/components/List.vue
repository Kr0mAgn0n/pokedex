<template>
  <v-container class="mb-8">
    <v-row v-for="(item, i) in items" :key="i">
      <Item
        :item="item"
        @pokemon-name="pokemonNameListener"
        @pokemon-fav="pokemonFavListener"
      ></Item>
    </v-row>

    <v-footer fixed>
      <v-layout justify-center class="mt-4">
        <v-btn
          class="mr-3 white--text"
          @click="onAllClick"
          :color="button_all_color"
          rounded
          width="150px"
          ><v-icon class="mr-3">fas fa-list</v-icon>All</v-btn
        >
        <v-btn
          class="white--text"
          @click="onFavsClick"
          rounded
          width="150px"
          :color="button_favorites_color"
          ><v-icon class="mr-3">fas fa-star</v-icon>Favorites</v-btn
        >
      </v-layout>
    </v-footer>

    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title class="world mb-3">
          <v-btn
            class="close-dialog-icon"
            absolute
            top
            right
            icon
            fab
            @click="closeDialog"
            ><v-icon>fa-times-circle</v-icon></v-btn
          >
          <v-layout align-center style="top: -56px">
            <v-img :src="pokemon_img" height="220px" contain></v-img>
          </v-layout>
        </v-card-title>

        <v-card-text>
          <v-list>
            <v-list-item>
              <span class="font-weight-bold">Name:</span
              ><span class="pokemon-details-name ml-3">{{ pokemon.name }}</span>
            </v-list-item>
            <v-list-item>
              <span class="font-weight-bold">Weight:</span
              ><span class="ml-3">{{ pokemon.weight }}</span>
            </v-list-item>
            <v-list-item>
              <span class="font-weight-bold">Height:</span
              ><span class="ml-3">{{ pokemon.height }}</span>
            </v-list-item>
            <v-list-item>
              <span class="font-weight-bold">Types:</span
              ><span class="ml-3">{{ pokemon_types_str }}</span>
            </v-list-item>
          </v-list>
          <v-layout justify-center>
            <v-btn class="mr-3 white--text" rounded color="#f22539"
              >Share to my friends</v-btn
            >
            <v-btn icon
              ><v-icon :color="dialog_star_color">fas fa-star</v-icon></v-btn
            >
          </v-layout>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import axios from "axios";
import Item from "../components/Item";
export default {
  name: "List",
  components: {
    Item,
  },
  props: {
    pokemons: Array,
    search_str: String,
  },
  data: () => {
    return {
      items: [],
      dialog: false,
      pokemon: {},
      pokemon_types: [],
      pokemon_img: "",
      favs: [],
      pokemons_: [],
      color1: "#f22539",
      color2: "#bfbfbf",
      button_all_color: "#f22539",
      button_favorites_color: "#bfbfbf",
    };
  },
  computed: {
    pokemon_types_str: function () {
      let str = "";
      this.pokemon_types.forEach((element) => {
        str += this.capitalize(element.type.name) + ", ";
      });
      return str.slice(0, -2);
    },

    dialog_star_color: function () {
      let color;
      this.pokemons_.forEach((element) => {
        if (element.name === this.pokemon.name) {
          if (element.fav) {
            color = "#ECA539";
          } else {
            color = "#BFBFBF";
          }
        }
      });
      return color;
    },
  },
  mounted: function () {
    // let _this = this;
    // axios.get('https://pokeapi.co/api/v2/pokemon').then(function(result){
    // // console.log(result);
    // _this.items = result.data.results;
    // // console.log(_this);
    // });
    // this.items = this.pokemons
    let _this = this;
    // console.log(_this.pokemons_)
    _this.pokemons.forEach((element) => {
      _this.pokemons_.push({
        name: element.name,
        fav: false,
      });
    });
    _this.items = _this.pokemons_;
  },
  updated: function () {
    // console.log("cambié");
  },
  watch: {
    search_str: function () {
    //   console.log("search_str cambió");
      this.items = [];
      this.pokemons_.forEach((element) => {
        if (element.name.includes(this.search_str)) {
          this.items.push(element);
        }
      });
    },

    // items: function () {
    //   console.log("items cambió");
    // },
  },
  methods: {
    // onNameClick: function(e) {
    //     // console.log(e.target.innerText);
    //     let name = e.target.innerHTML;
    //     let _this = this;
    //     axios.get('https://pokeapi.co/api/v2/pokemon/' + name).then(function(result) {
    //         // console.log(result);
    //         _this.pokemon = result.data;
    //         _this.pokemon_types = result.data.types;
    //         _this.pokemon_img = result.data.sprites.other["official-artwork"].front_default
    //         console.log(_this.pokemon_img)
    //     });
    //     // console.log(this);
    //     this.dialog = true;
    // },

    capitalize: function (str) {
      const lower = str.toLowerCase();
      return str.charAt(0).toUpperCase() + lower.slice(1);
    },

    closeDialog: function () {
      this.dialog = false;
    },

    pokemonNameListener: function (pokemon_name) {
      // console.log(pokemon_name)
      let name = pokemon_name;
      let _this = this;
      axios
        .get("https://pokeapi.co/api/v2/pokemon/" + name)
        .then(function (result) {
          // console.log(result);
          _this.pokemon = result.data;
          _this.pokemon_types = result.data.types;
          _this.pokemon_img =
            result.data.sprites.other["official-artwork"].front_default;
          // console.log(_this.pokemon_img)
        });
      // console.log(this);
      this.dialog = true;
    },

    pokemonFavListener: function (info) {
      this.pokemons_.forEach((element) => {
        if (element.name === info.name) {
          element.fav = info.selected;
        }
      });
    },

    onFavsClick: function () {
      let _this = this;
      _this.items = [];
      _this.pokemons_.forEach((element) => {
        if (element.fav) {
          _this.items.push(element);
        }
      });
      _this.button_all_color = _this.color2;
      _this.button_favorites_color = this.color1;
    },

    onAllClick: function () {
      this.items = [];
      this.items = this.pokemons_;
      this.button_all_color = this.color1;
      this.button_favorites_color = this.color2;
    },
  },
};
</script>

<style scoped>
.item {
  border: 2px solid black;
}

.nombre {
  text-transform: capitalize;
  width: 80%;
}

.world {
  background-image: url("../assets/img/world.png");
  background-size: auto 220px;
  background-repeat: repeat-x;
  height: 220px;
  width: 100%;
}

.close-dialog-icon {
  top: 10px !important;
}

.pokemon-details-name {
  text-transform: capitalize;
}

.pokemon-img {
  height: 220px;
  width: auto;
}

.loading-img {
  /* position: absolute;
    top: 50%;
    left: 50%; */
}
</style>