<template>
  <div class="title">
    <img class="icon flip" alt="covid-19 logo" src="./assets/covid_icon.png" />
    <h1>COVID-19 Research</h1>
    <img class="icon" alt="covid-19 logo" src="./assets/covid_icon.png" />
  </div>
  <show-document v-if="!!doc" v-bind:doc="doc" />
  <search v-else></search>
</template>

<script>
import Search from "./components/Search.vue";
import ShowDocument from "./components/ShowDocument.vue";

const HOST = "http://localhost:9200";
const API_ROUTE = "covid_research/_doc";

export default {
  created() {
    if (window.location.search.indexOf("doc=") > -1) {
      let param = window.location.search.slice(
        window.location.search.indexOf("doc=") + 4
      );
      this.getapi(param).then((value) => {
        console.log(value);
        if (value.found) {
          this.doc = {
            title: value._source.title,
            published: value._source["published date"],
            authors: value._source.authors,
            abstract: value._source.abstract,
            url: value._source.url,
          };
        }
      });
    }
  },
  data() {
    return {
      doc: null,
    };
  },
  name: "App",
  methods: {
    async getapi(id) {
      const response = await fetch(`${HOST}/${API_ROUTE}/${id}`, {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
        mode: "cors",
        credentials: "same-origin",
      });
      return response.json();
    },
  },
  components: {
    Search,
    ShowDocument,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #2c3e50; */
  margin: 20px 60px 0px 60px;
}
.title {
  font-size: large;
  display: flex;
  justify-content: center;
}
.icon {
  width: 60px;
  height: 60px;
  padding: 20px;
}
.flip {
  transform: scaleX(-1);
}
</style>
