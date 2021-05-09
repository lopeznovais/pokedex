<template>
  <div class="mt-5">
    <p>{{ this.pokemon.description | removeArrow }}</p>
    <br />
    <div class="card-image">
      <figure>
        <img :src="back" alt="Placeholder image" />
      </figure>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  created: function() {
    axios.get(this.url).then((res) => {
      this.pokemon.description = res.data.flavor_text_entries[0].flavor_text;
    });
  },
  data() {
    return {
      pokemon: {
        description: "",
      },
    };
  },
  filters: {
    removeArrow: function(value) {
      var newName = value.replace("", " ");
      return newName;
    },
  },
  props: {
    url: String,
    back: String,
  },
};
</script>

<style></style>
