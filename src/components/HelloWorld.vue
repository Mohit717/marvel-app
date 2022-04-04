<template>
  <div class="hello">
    <HeaderComponent />
    <main role="main">
      <!-- <JumboTron /> -->
      <div class="album py-5 bg-light">
        <div class="container">
          <div class="row">
            <div class="col-md-3 mb-3">
              <select class="form-control" @change="sortCharacter($event)">
                <option selected value="name">Name ASC</option>
                <option value="-name">Name DESC</option>
                <option value="modified">Modified ASC</option>
                <option value="-modified">Modified DESC</option>
              </select>
            </div>
          </div>

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
                        data-toggle="modal"
                        data-target="#myModal2"
                      >
                        View
                      </button>
                    </div>
                    <small class="text-muted">{{
                      formatDate(character.modified)
                    }}</small>
                  </div>
                </div>
              </div>
            </div>
            <div v-if="loading" class="loader">
              <img src="@/assets/loader.svg" />
            </div>
          </div>
        </div>
      </div>
    </main>
    <FooterComponent />
    <RightModalComponent />
  </div>
</template>

<script>
import axios from "axios";
import HeaderComponent from "./HeaderComponent.vue";
import JumboTron from "./Jumbotron.vue";
import FooterComponent from "./FooterComponent.vue";
import RightModalComponent from "./RightModalComponent.vue"

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  components: {
    HeaderComponent,
    JumboTron,
    FooterComponent,
    RightModalComponent
  },
  data() {
    return {
      siteName: process.env.VUE_APP_TITLE,
      characters: [],
      perpage: 12,
      offset: 0,
      orderBy: "name",
      loading: false,
    };
  },
  methods: {
    async getInitialCharacters() {
      this.loading = true;
      await axios
        .get(
          `${process.env.VUE_APP_API_URL}characters?orderBy=${this.orderBy}&limit=${this.perpage}&offset=${this.offset}&apikey=${process.env.VUE_APP_API_KEY}`
        )
        .then((response) => {
          this.loading = false;
          response.data.data.results.map((character) => {
            this.characters.push(character);
          });
          console.log(this.characters)
          this.offset = response.data.data.offset;
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        });
    },
    getNextUser() {
      window.onscroll = () => {
        let bottomOfWindow =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;
        if (bottomOfWindow) {
          this.offset = this.offset + this.perpage;
          this.getInitialCharacters();
        }
      };
    },
    sortCharacter(event) {
      this.orderBy = event.target.value;
      this.characters = [];
      this.getInitialCharacters();
    },
    formatDate(dateString) {
      let convertedDate = new Date(dateString);
      return convertedDate.toDateString();
    },
  },
  beforeMount() {
    this.getInitialCharacters();
  },
  mounted() {
    this.getNextUser();
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
.card-img-top {
  height: 250px;
}
.loader {
  width: 100px;
  margin: auto;
}
.loader img {
  width: 100px;
}
</style>
