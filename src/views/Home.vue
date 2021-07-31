<template>
  <v-container class="container">
    <v-text-field class="search mb-8" prepend-inner-icon="fa-search" placeholder="Search" solo>
    </v-text-field>
    <List v-if="items.length" :pokemons="items"></List>
    <EmptyList v-else></EmptyList> 
    <v-overlay :value="loading" color="#ffffff" opacity="1">
      <v-img src="../assets/img/loading.png">
      </v-img>
    </v-overlay>     
  </v-container>
</template>

<script>
  import EmptyList from '../components/EmptyList'
  import List from '../components/List'
  import axios from 'axios'

  export default {
    name: 'Home',

    components: {
      EmptyList,
      List
    },

    data: function() {
      return {
        items:[],
        loading: true
      };
    },
    mounted: function(){
      let _this = this;
      axios.get('https://pokeapi.co/api/v2/pokemon').then(function(result){
        // console.log(result);
        _this.items = result.data.results;
        // console.log(_this);
      });
      setTimeout(function(){
          _this.loading = false
        },2000)
    }
  }

</script>

<style scoped>
.container {
  max-width: 1152px;
}
</style>
