<template>
  <div class="col-md-3" v-for="(character, index) in characters" :key="index">
    <div class="card mb-4 box-shadow">
      <img
        class="card-img-top"
        :src="character.thumbnail.path + '.' + character.thumbnail.extension"
        alt="Card image cap"
      />
      <div class="card-body">
        <small>{{ formatDate(character.modified) }}</small>
        <p class="card-text">
          {{ character.name }}
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
  props: {
    offset
  },
  data() {
    return {
      siteName: process.env.VUE_APP_TITLE,
      data: [],
      characters: [],
      perpage: 12,
    };
  },
  methods: {
    async getInitialCharacters() {
      await axios
        .get(
          `${process.env.VUE_APP_API_URL}characters?orderBy=name&limit=${this.perpage}&offset=${this.offset}&apikey=${process.env.VUE_APP_API_KEY}`
        )
        .then((response) => {
          console.log(response)
          this.data = response.data.data;
          this.$emit("pagedata", this.data);
          this.characters = response.data.data.results;
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        });
    },
    formatDate(dateString) {
      let convertedDate = new Date(dateString);
      return convertedDate.toDateString();
    }
  },
  beforeMount() {
    this.getInitialCharacters();
  },
};
</script>
