<template>
    <div class="welcome">
        <div class="image">
            <img src="../assets/img/pikachu.png" alt="">
        </div>
        <h2 class="mb-5">Welcome to Pokédex</h2>
        <p>The digital encyclopedia created by Professor Oak is an invaluable tool to Trainers in the Pokémon world.</p>
        <v-btn class="white--text" href="/home" :color="btn_color" rounded width="150px" @click="onBtnClick" :ripple="false">Get started</v-btn>
        
    </div>
</template>
<script>
export default {
    name: 'Welcome',
    data: function(){
        return {
            color1: '#f22539',
            color2: '#c00e20',
            btn_color: '#f22539'
        }
    },
    methods: {
        onBtnClick: function(){
            let _this = this
            _this.btn_color = this.color2
            setTimeout(function(){
                _this.btn_color = this.color1
            }, 500)
        }
    }
}
</script>

<style scoped>
.welcome {
    text-align: center;
}

/* .image {
    text-align: center;
} */

.image img {
    width: 325px;
    height: auto;
}

p {
    font-size: 18px;
}

h2 {
    font-size: 26px;
}
</style>>