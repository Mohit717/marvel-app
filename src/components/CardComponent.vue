<template>
  <div class="col-md-4" v-for="(character, index) in characters" :key="index">
    <div class="card mb-4 box-shadow">
      <img
        class="card-img-top"
        :src="character.thumbnail.path+'.'+character.thumbnail.extension"
        alt="Card image cap"
      />
      <div class="card-body">
        <p class="card-text">
          {{character.name}}
        </p>
        <div class="d-flex justify-content-between align-items-center">
          <div class="btn-group">
            <button type="button" class="btn btn-sm btn-outline-secondary">
              View
            </button>
            <button type="button" class="btn btn-sm btn-outline-secondary">
              Edit
            </button>
          </div>
          <small class="text-muted">9 mins</small>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CardComponent",
  data() {
    return {
      siteName: process.env.VUE_APP_TITLE,
      data: [],
      characters: [],
    };
  },
  async mounted() {
    await axios
      .get(
        `${process.env.VUE_APP_API_URL}characters?apikey=${process.env.VUE_APP_API_KEY}`
      )
      .then((response) => {
        this.data = response.data.data;
        this.characters = response.data.data.results;
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      });
  },
};
</script>
