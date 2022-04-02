<template>
  <div class="hello">
    <HeaderComponent />
    <main role="main">
      <JumboTron />

      <div class="album py-5 bg-light">
        <div class="container">
          <div class="row">
            <CardComponent :offset="offset" @pagedata="pagedata" ref="cardcop"/>
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
import HeaderComponent from "./HeaderComponent.vue";
import JumboTron from "./Jumbotron.vue";
import CardComponent from "./CardComponent.vue";
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
    CardComponent,
    PaginationComponent,
    FooterComponent,
  },
  data() {
    return {
      siteName: process.env.VUE_APP_TITLE,
      currentPage: 1,
      totalPage: null,
      total:null,
      perpage:12,
      offset:null,
    };
  },
  methods: {
    onPageChange(page) {
      this.currentPage = page;
      this.$refs.cardcop.getInitialCharacters();
    },
    pagedata(data){
      this.total = data.total
      this.perpage = data.limit
      this.totalPage = data.total / data.limit
      this.offset = (data.limit*this.currentPage) - data.limit
    },
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
