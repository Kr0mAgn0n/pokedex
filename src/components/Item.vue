<template>
    <v-row class="pokemon mb-5" align="center">
        <v-col @click="onNameClick">
            {{item_str}}
        </v-col>
        <v-col cols="1">
            <v-layout justify-end>
                <v-btn icon @click="onPokemonFav">
                    <v-icon ref="pokemon-fav" :color="color">fas fa-star</v-icon>
                </v-btn>
            </v-layout>
        </v-col>
    </v-row>
</template>

<script>
export default {
    name: 'Item',
    props: {
        item: Object
    },
    data: function(){
        return {
            selected: Boolean,
        }
    },
    computed: {
        item_str: function (){
            return this.capitalize(this.item.name)
        },

        color: function(){
            if (this.selected) {
                return '#ECA539'
            } else {
                return '#BFBFBF'
            }
        }
    },
    mounted: function(){
        this.selected = this.item.fav
        // console.log(this.selected)
    },
    updated: function(){
        this.selected = this.item.fav
        // console.log(this.selected)
    },
    methods: {
        capitalize: function(str) {
            const lower = str.toLowerCase()
            return str.charAt(0).toUpperCase() + lower.slice(1)
        },

        onNameClick: function (){
            this.$emit('pokemon-name', this.item.name)
        },

        onPokemonFav: function (){
            // console.log(this.$refs['pokemon-fav'])
            this.selected = !this.selected
            
            
            this.$emit('pokemon-fav', {
                name: this.item.name,
                selected: this.selected
            })
        }
    }
}
</script>

<style scope>
.pokemon {
    background-color: #ffffff;
    border-radius: 5px;
}
</style>