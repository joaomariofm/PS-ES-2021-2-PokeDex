<template>
  <div v-if="isFetched" class="back-card-line">

    <div :class="['card-fill', pokemon.types[0].type.name]">
      <div class="name-n-power">
        <h1 class="fs-6 mt-2">{{ pokemonNameUpperCase(pokemon.name) }}</h1>
        <div class="powers d-flex align-items-center">
          <img
            draggable="false"
            v-for="element in pokemon.types"
            :key="element.slot"
            id="type-icon"
            :src="require(`../assets/type-icons/${element.type.name}.svg`)"
          />
          <h2 class="fs-6 mb-0 me-2">{{ pokemon.stats.hp + " HP" }}</h2>
        </div>
      </div>

      <div class="pokemon-display-back-line">
        <div class="pokemon-display">
          <img draggable="false" :src="pokemon.img" :alt="pokemon.nome" />
        </div>
      </div>

      <div class="pokemon-stats">
        <div class="stat">
          <h1>Attack</h1>
          <p>{{ pokemon.stats.attack }}</p>
        </div>
        <div class="stat">
          <h1>Defense</h1>
          <p>{{ pokemon.stats.defense }}</p>
        </div>
        <div class="stat">
          <h1>Special-Attack</h1>
          <p>{{ pokemon.stats.special_attack }}</p>
        </div>
        <div class="stat">
          <h1>Special-Defense</h1>
          <p>{{ pokemon.stats.special_defense }}</p>
        </div>
        <div class="stat">
          <h1>Speed</h1>
          <p>{{ pokemon.stats.speed }}</p>
        </div>
      </div>
    </div>
  </div>
  <div v-else class="loader-card">
    <div class="c-loader"></div>
  </div>
</template>

<script>
import pokeapi from '@/http/pokeapi'

export default {
  props: {
    url: {
      type: String,
      required: true,
    }
  },
  watch: {
    url(){
      if(this.url !== 'null') {
        pokeapi.get(this.url)
        .then((res) => {
          this.pokemon.name = res.data.name;
          this.pokemon.id = res.data.id;
          this.pokemon.type = res.data.types[0].type.name;
          this.pokemon.types = res.data.types;
          this.pokemon.img = res.data.sprites.front_default;
          this.pokemon.stats.hp = res.data.stats[0].base_stat;
          this.pokemon.stats.attack = res.data.stats[1].base_stat;
          this.pokemon.stats.defense = res.data.stats[2].base_stat;
          this.pokemon.stats.special_attack = res.data.stats[3].base_stat;
          this.pokemon.stats.special_defense = res.data.stats[4].base_stat;
          this.pokemon.stats.speed = res.data.stats[5].base_stat;

          this.isFetched = true;
        })
        .catch((err) => console.log(err));
      } else {
        this.isFetched = false;
      }
    }
  },
  data() {
    return {
      isFetched: false,
      pokemon: {
        img: "",
        name: "",
        id: -1,
        types: [],
        type: "",
        stats: {
          hp: "",
          attack: "",
          defense: "",
          special_attack: "",
          special_defense: "",
          speed: "",
        },
      },
    };
  },
  methods: {
    pokemonNameUpperCase(name) {
      return name.charAt(0).toUpperCase() + name.slice(1);
    },
  }
}
</script>

<style scoped lang='stylus'>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap");

* 
  color: black;
  font-family: "Roboto", sans-serif;
  font-weight: 500;
  .back-card-line
    position: relative;
    margin: 25px;
    width: 220px;
    height: 280px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #ffff00;
    box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.25);
    border-radius: 4px;
    border: 1.5px solid #000000;
    .fav-container 
      position: absolute;
      top: -31px;
      left: 2px;
      display: flex;
      align-items: center;
      justify-content: center;
      width: 30px;
      height: 28px;
      border-radius: 5px 5px 0px 0px;
      border: .5px solid #000000;
      box-shadow: 2px -2px 40px rgba(0, 0, 0, 0.15);
      background-color: #FFF;
      cursor: pointer;
      img 
        margin-bottom: 2px;
        width: auto;
        height: 20px;
    .card-fill 
      width: 209px;
      height: 266px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      border-radius: 8px;
      border: 1.5px solid #000000;
      .name-n-power 
        width: 200px;
        margin-bottom: 3px;
        display: grid;
        grid-template-columns: 50% 50%;
        align-items: center;
        justify-content: space-between;
        h1 
          margin-left: 10px;
        .powers 
          width: 100%;
          height: 100%;
          display: flex;
          flex-direction: row-reverse;
          align-items: center;
          justify-content: flex-start;
          h2 
            margin-right: 5px;
            font-size: 12px;
          #type-icon 
            margin-left: 2px;
            margin-right: 2px;
            width: 20px;
            height: 25px;
      .pokemon-display-back-line 
        width: 179px;
        height: 115px;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: #ffff00;
        border-radius: 2px;
        border: 1.5px solid #000000;
        .pokemon-display 
          width: 170px;
          height: 108px;
          display: flex;
          align-items: center;
          justify-content: center;
          background-color: #ffffff;
          border-radius: 2px;
          border: 1.5px solid #000000;
          box-shadow: rgba(50, 50, 93, 0.25) 0px 30px 60px -12px inset,
            rgba(0, 0, 0, 0.3) 0px 18px 36px -18px inset;
      .pokemon-stats 
        padding: 10px 0px 10px 0px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        .stat 
          width: 179px;
          height: 15px;
          display: flex;
          align-items: center;
          justify-content: space-between;
          margin: 2px 0px 2px 0px;
          background-color: #ffffff;
          border-radius: 3px;
          box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
          h1 
            margin: 0 0 0 3px;
            font-size: 11px;
          p 
            margin: 0 3px 0 0;
            font-size: 11px;


    .normal
      background: rgb(168, 167, 122);
      background: linear-gradient(
        0deg,
        rgba(168, 167, 122, 1) 0%,
        rgba(252, 239, 239, 1) 100%
      );
    .grass
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #7ac74c;
    .fire 
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #ee8130;
    .water
      background: linear-gradient(
          180deg,
          rgba(251, 249, 249, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #6390f0;
    .bug
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #a6b91a;
    .poison
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #a33ea1;
    .electric
      background: linear-gradient(
          180deg,
          rgba(251, 249, 249, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #f7d02c;
    .ground
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #e2bf65;
    .fairy
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #d685ad;
    .fighting
      background: linear-gradient(
          180deg,
          rgba(251, 249, 249, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #bbaacc;
    .psychic
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #f95587;
    .rock
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #b6a136;
    .ghost
      background: linear-gradient(
          180deg,
          rgba(251, 249, 249, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #735797;
    .ice
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #96d9d6;
    .flying
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #a98ff3;
    .steel
      background: linear-gradient(
          180deg,
          rgba(251, 249, 249, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #b7b7ce;
    .dark
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #705746;
    .dragon
      background: linear-gradient(
          180deg,
          rgba(252, 239, 239, 0.2) 0%,
          rgba(0, 0, 0, 0) 100%
        ),
        #6f35fc;


.loader-card
  margin: 25px;
  width: 220px;
  height: 280px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #C4C4C4;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.25);
  border-radius: 4px;
  border: 1.5px solid #000000;
  .c-loader 
    animation: is-rotating 1s infinite;
    border: 6px solid #e5e5e5;
    border-radius: 50%;
    border-top-color: gray;
    width: 120px;
    height: 120px;
  

@keyframes is-rotating {
  to {
    transform: rotate(1turn);
  }
}
</style>