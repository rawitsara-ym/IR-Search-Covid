<template>
  <basic-search v-if="!advancedSearch" v-model="query"></basic-search>
  <advance-search v-else v-model="query"></advance-search>
  <textarea id="show_query" v-model="showQuery" readonly></textarea>
</template>

<script>
import AdvanceSearch from "./AdvanceSearch.vue";
import BasicSearch from "./BasicSearch.vue";

const api = "covid_research/_search";

function pretty_json(object, indent = 0) {
  let str = "{\n";
  indent += 1;
  const array_str = [];
  for (const key in object) {
    if (Object.hasOwnProperty.call(object, key)) {
      const element = object[key];
      const startwith = "  ".repeat(indent) + `"${key}": `;
      if (Array.isArray(element)) {
        if (element.length > 0) {
          array_str.push(
            `${startwith}[${element.reduce(
              (prev, curr) => `${prev}, ${curr}`
            )}]`
          );
        } else {
          array_str.push(`${startwith}[]`);
        }
      } else if (typeof element === "object") {
        array_str.push(startwith + pretty_json(element, indent));
      } else if (typeof element === "string") {
        array_str.push(`${startwith}"${element}"`);
      } else {
        array_str.push(`${startwith}${element}`);
      }
    }
  }
  if (array_str.length > 0) {
    str += array_str.reduce((prev, curr) => {
      prev + ",\n" + curr;
    });
  }
  indent -= 1;
  str += "\n" + "  ".repeat(indent) + "}";
  return str;
}

export default {
  data() {
    return { advancedSearch: true, query: {} };
  },
  components: {
    BasicSearch,
    AdvanceSearch,
  },
  computed: {
    showQuery() {
      let displayQuery = `GET ${api}\n`;
      displayQuery += pretty_json({ query: this.query });
      return displayQuery;
    },
  },
};
</script>

<style scoped>
textarea {
  width: 580px;
  height: 150px;
  padding: 12px 20px;
  box-sizing: border-box;
  border: 2px solid #ccc;
  border-radius: 4px;
  background-color: #f8f8f8;
  font-size: 16px;
  resize: none;
}
</style>