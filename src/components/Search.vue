<template>
  <advance-search
    v-if="advancedSearch"
    v-model="query"
    @callback="getapi()"
  ></advance-search>
  <template v-else>
    <basic-search v-model="query" @callback="getapi()"></basic-search>
    <div>
      <a href="javascript:void(0)" @click="() => (advancedSearch = true)"
      >Advanced Search</a>
    </div>
  </template>
  <textarea
    id="show_query"
    v-model="showQuery"
    readonly
    cols="80"
    rows="20"
  ></textarea>
  <div v-if="result!=null">
    <p v-for="(value,index) in result.hits.hits" :key="index"> {{value._source.title}} </p>
  </div>
</template>

<script>
import AdvanceSearch from "./AdvanceSearch.vue";
import BasicSearch from "./BasicSearch.vue";

const HOST = "http://localhost:9200";
const API_ROUTE = "covid_research/_search";

export default {
  data() {
    return { advancedSearch: false, query: {}, result: null };
  },
  components: {
    BasicSearch,
    AdvanceSearch,
  },
  methods: {
    async getapi() {
      const result = await fetch(`${HOST}/${API_ROUTE}`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        mode: "cors",
        credentials: "same-origin",
        body: JSON.stringify(this.finalQuery),
      });
      this.result = result;
    },
  },
  computed: {
    finalQuery() {
      return { size: 250, query: this.query };
    },
    showQuery() {
      let displayQuery = `GET ${API_ROUTE}\n`;
      displayQuery += JSON.stringify(this.finalQuery, null, 2);
      return displayQuery;
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
</style>