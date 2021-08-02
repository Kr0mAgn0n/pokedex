<template>
  <v-container class="container">
    <v-text-field
      class="search mb-8"
      prepend-inner-icon="fa-search"
      placeholder="Search"
      solo
      @keyup="onSearchKeyUp"
      ref="search"
    >
    </v-text-field>
    <List
      ref="list"
      v-if="items.length"
      :pokemons="items"
      :search_str="search_str"
    ></List>
    <EmptyList v-else></EmptyList>
    <v-overlay :value="loading" color="#ffffff" opacity="1">
      <v-img class="loading-img" src="../assets/img/loading.png"> </v-img>
    </v-overlay>
  </v-container>
</template>

<script>
import EmptyList from "../components/EmptyList";
import List from "../components/List";
import axios from "axios";

export default {
  name: "Home",

  components: {
    EmptyList,
    List,
  },

  data: function () {
    return {
      items: [],
      loading: true,
      search_str: "",
    };
  },
  mounted: function () {
    let _this = this;
    axios.get("https://pokeapi.co/api/v2/pokemon").then(function (result) {
      // console.log(result);
      _this.items = result.data.results;
      // console.log(_this);
    });
    setTimeout(function () {
      _this.loading = false;
    }, 2000);
  },
  methods: {
    onSearchKeyUp: function (e) {
      // console.log(e);
      if (e.key === "Enter") {
        // console.log("Se presionó Enter");
        // console.log(this.$refs["search"].lazyValue);
        // let search_str = this.search_str;
        this.search_str = this.$refs["search"].lazyValue;
        // let list_pokemons = this.$refs["list"].pokemons;
        // let list_items = this.$refs["list"].items;
        // console.log(search_str);
        // console.log(list_pokemons);
        // console.log(list_items);
        // list_items = [];
        // list_pokemons.forEach((element) => {
        //   if (element.name.includes(this.search_str)) {
        //     list_items.push(element);
        //   }
        // });
        // console.log(list_items);
      }
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 1152px;
}

.loading-img {
  animation: fadeInFromNone 2s linear;
}

@keyframes fadeInFromNone {
  0% {
    display: block;
    opacity: 1;
  }

  25% {
    display: block;
    opacity: 0.5;
  }

  50% {
    display: block;
    opacity: 0.25;
  }

  75% {
    display: block;
    opacity: 0.5;
  }

  100% {
    display: block;
    opacity: 1;
  }
}
</style>
