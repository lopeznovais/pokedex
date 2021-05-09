<template>
  <div id="pokemon">
    <div class="card">
      <div class="columns">
        <div class="column" v-bind:class="{ 'is-half': showMore }">
          <div class="card-image">
            <figure>
              <img :src="currentImg" alt="Placeholder image" />
            </figure>
          </div>
          <div class="card-content">
            <div class="media">
              <div class="media-content">
                <p class="title is-4">{{ num }} - {{ name | upper }}</p>
                <template v-for="type in pokemon.types">
                  {{ type.type.name | upper }}
                </template>
              </div>
            </div>
          </div>
        </div>
        <div class="card-content column is-half" v-if="showMore">
          <Info :url="pokemon.url" :back="pokemon.back" />
        </div>
      </div>
      <div class="card-content">
        <button class="button is-medium is-fullwidth" @click="changeShowMore">
          Ver Mais
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Info from "./Info";
export default {
  created: function() {
    axios.get(this.url).then((res) => {
      this.pokemon.types = res.data.types;
      this.pokemon.front = res.data.sprites.front_default;
      this.pokemon.back = res.data.sprites.back_default;
      this.pokemon.url = res.data.species.url;
      this.currentImg = this.pokemon.front;
    });
  },
  data() {
    return {
      showMore: false,
      isFront: true,
      currentImg: "",
      pokemon: {
        type: "",
        front: "",
        back: "",
        url: "",
      },
    };
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  filters: {
    upper: function(value) {
      var newName = value[0].toUpperCase() + value.slice(1);
      return newName;
    },
  },
  methods: {
    mudarSprite: function() {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.pokemon.back;
      } else {
        this.isFront = true;
        this.currentImg = this.pokemon.front;
      }
    },
    changeShowMore: function() {
      if (this.showMore) {
        this.showMore = false;
      } else {
        this.showMore = true;
      }
    },
  },
  components: {
    Info,
  },
};
</script>

<style>
#pokemon {
  margin-top: 2%;
}
</style>
