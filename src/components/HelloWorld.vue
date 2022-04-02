<template>
  <div class="hello">
    <HeaderComponent />
    <main role="main">
      <JumboTron />

      <div class="album py-5 bg-light">
        <div class="container">
          <div class="row">
            <div
              class="col-md-3"
              v-for="(character, index) in characters"
              :key="index"
            >
              <div class="card mb-4 box-shadow">
                <img
                  class="card-img-top"
                  :src="
                    character.thumbnail.path +
                    '.' +
                    character.thumbnail.extension
                  "
                  alt="Card image cap"
                />
                <div class="card-body">
                  <small>{{ formatDate(character.modified) }}</small>
                  <p class="card-text">
                    {{ character.name }}
                  </p>
                  <div
                    class="d-flex justify-content-between align-items-center"
                  >
                    <div class="btn-group">
                      <button
                        type="button"
                        class="btn btn-sm btn-outline-secondary"
                      >
                        View
                      </button>
                      <button
                        type="button"
                        class="btn btn-sm btn-outline-secondary"
                      >
                        Edit
                      </button>
                    </div>
                    <small class="text-muted">9 mins</small>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="row">
            <PaginationComponent
              :total-pages="totalPage"
              :total="total"
              :per-page="perpage"
              :current-page="currentPage"
              @pagechanged="onPageChange"
            />
          </div>
        </div>
      </div>
    </main>

    <FooterComponent />
  </div>
</template>

<script>
import axios from "axios";
import HeaderComponent from "./HeaderComponent.vue";
import JumboTron from "./Jumbotron.vue";
import PaginationComponent from "./PaginationComponent.vue";
import FooterComponent from "./FooterComponent.vue";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  components: {
    HeaderComponent,
    JumboTron,
    PaginationComponent,
    FooterComponent,
  },
  data() {
    return {
      siteName: process.env.VUE_APP_TITLE,
      characters: [],
      perpage: 12,
      currentPage: 1,
      totalPage: null,
      total: null,
      perpage: 12,
      offset: 0,
    };
  },
  methods: {
    onPageChange(page) {
      this.currentPage = page;
      this.offset = page * this.perpage - this.perpage;
      this.getInitialCharacters();
    },
    async getInitialCharacters() {
      await axios
        .get(
          `${process.env.VUE_APP_API_URL}characters?orderBy=name&limit=${this.perpage}&offset=${this.offset}&apikey=${process.env.VUE_APP_API_KEY}`
        )
        .then((response) => {
          this.total = response.data.data.total;
          this.perpage = response.data.data.limit;
          this.totalPage = response.data.data.total / response.data.data.limit;
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
    },
  },
  beforeMount() {
    this.getInitialCharacters();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
