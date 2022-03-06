<template>
  <div class="hello">
    <HeaderComponent />
    <main role="main">
      <JumboTron />

      <div class="album py-5 bg-light">
        <div class="container">
          <div class="row">
            <CardComponent />
          </div>
        </div>
      </div>
    </main>

    <FooterComponent />
  </div>
</template>

<script>
import HeaderComponent from './HeaderComponent.vue'
import JumboTron from './Jumbotron.vue'
import CardComponent from './CardComponent.vue'
import FooterComponent from './FooterComponent.vue'
import axios from "axios";
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  components:{
    HeaderComponent,
    JumboTron,
    CardComponent,
    FooterComponent
  },
  data() {
    return {
      siteName: process.env.VUE_APP_TITLE,
      data: [],
      results: [],
    };
  },
  async mounted() {
    await axios
      .get(
        `${process.env.VUE_APP_API_URL}characters?apikey=${process.env.VUE_APP_API_KEY}`
      )
      .then((response) => {
        this.data = response.data.data;
        this.results = response.data.data.results;
      })
      .catch((error) => {
        console.log(error);
        this.errored = true;
      });
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
