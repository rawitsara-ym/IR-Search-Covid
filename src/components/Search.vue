<template>
  <div style="margin: 20px">
    <a
      href="javascript:void(0)"
      @click="() => (advancedSearch = !advancedSearch)"
      >{{ "Go to " + (advancedSearch ? "Basic Search" : "Advanced Search") }}</a
    >
  </div>
  <advance-search
    v-if="advancedSearch"
    v-model="query"
    @callback="this.fetchData()"
  ></advance-search>
  <basic-search
    v-else
    v-model="query"
    @callback="this.fetchData()"
  ></basic-search>
  <textarea
    id="show_query"
    v-model="showQuery"
    readonly
    cols="80"
    rows="20"
  ></textarea>
  <div v-if="result?.hits" style="margin: 50px">
    <p style="margin-bottom : 30px;font-weight:bold;">{{amount}}</p>
    <div v-for="value in result.hits.hits" :key="value._id">
      <p class="section">
        <a :href="`?doc=${value._id}`">{{ value._source.title }}</a>
      </p>
      <hr />
    </div>
  </div>
</template>

<script>
import AdvanceSearch from "./AdvanceSearch.vue";
import BasicSearch from "./BasicSearch.vue";

const HOST = "http://localhost:9200";
const API_ROUTE = "covid_research/_search";

export default {
  data() {
    return { advancedSearch: false, query: null, result: null };
  },
  components: {
    BasicSearch,
    AdvanceSearch,
  },
  methods: {
    async postapi(body) {
      const response = await fetch(`${HOST}/${API_ROUTE}`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        mode: "cors",
        credentials: "same-origin",
        body: JSON.stringify(body),
      });
      return response.json();
    },
    async fetchData() {
      this.result = await this.postapi(this.finalQuery);
    },
  },
  computed: {
    finalQuery() {
      return { size: 277, query: this.query };
    },
    showQuery() {
      let displayQuery = `POST ${API_ROUTE}\n`;
      displayQuery += JSON.stringify(this.finalQuery, null, 2);
      return displayQuery;
    },
    amount() {
      if (this.result?.hits) {
        if (this.result.hits.hits.length == 0) return `Not Found document.`;
        else if (this.result.hits.hits.length == 1) return `Found 1 document.`;
        else return `Found ${this.result.hits.hits.length} documents.`;
      }
      return "";
    },
  },
};
</script>

<style scoped>
textarea {
  width: 580px;
  height: 200px;
  padding: 12px 20px;
  box-sizing: border-box;
  border: 2px solid #ccc;
  border-radius: 4px;
  background-color: #f8f8f8;
  font-size: 16px;
  resize: none;
}
.section {
  text-align: left;
}
.section a {
  color: black;
  text-decoration: none;
}

.section a:hover {
  cursor: pointer;
  color: red;
  text-decoration: underline;
}
</style>
