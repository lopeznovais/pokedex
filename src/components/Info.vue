<template>
  <div class="mt-5">
    <p>{{ this.pokemon.description }}</p>
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
  props: {
    url: String,
  },
};
</script>

<style></style>
