<template>
  <div class="container">
    <form @submit.prevent="submit">
      <div class="search-container">
        <div class="grid-template-2">
          <label for="title">Title</label>
          <input
            type="text"
            id="title_search"
            name="title"
            v-model="title"
            @input="update_query()"
          />
          <label for="author">Author</label>
          <input
            type="text"
            id="author_search"
            name="author"
            v-model="author"
            @input="update_query()"
          />
        </div>
        <div class="grid-template-3">
          <label for="published_date">Published</label>
          <input
            type="date"
            id="published_date_from_search"
            name="published_date[from]"
            v-model="published.from"
            @input="update_query()"
          />
          <input
            type="date"
            id="published_date_to_search"
            name="published_date[to]"
            v-model="published.to"
            @input="update_query()"
          />
          <label for="url">URL</label>
          <input
            type="text"
            id="url_search"
            name="url"
            v-model="url"
            @input="update_query()"
          />
        </div>
        <div class="grid-template-1">
          <label for="abstract">Abstract</label>
          <input
            type="text"
            id="abstract_search"
            name="abstract"
            v-model="abstract"
            @input="update_query()"
          />
        </div>
      </div>
      <button type="submit">Search <i class="fas fa-search"></i></button>
    </form>
  </div>
</template>
<script>
export default {
  mounted() {
    this.$emit("update:modelValue", { bool: { must: [] } });
  },
  props: ["modelValue"],
  emits: ["update:modelValue", "callback"],
  data() {
    return {
      title: "",
      author: "",
      published: { from: "", to: "" },
      url: "",
      abstract: "",
    };
  },
  methods: {
    submit() {
      this.$emit("callback");
    },
    update_query() {
      const model = { bool: { must: [] } };
      if (this.title !== "") {
        model.bool.must.push({ match: { title: this.title } });
      }
      if (this.author !== "") {
        model.bool.must.push({ match: { authors: this.author } });
      }
      if (this.published.from !== "" || this.published.to !== "") {
        let published = { range: {} };
        if (this.published.from !== "") {
          published["range"]["gte"] = this.published.from;
        }
        if (this.published.to !== "") {
          published["range"]["lte"] = this.published.to;
        }
        console.log(published);
        model.bool.must.push(published);
      }
      if (this.url !== "") {
        model.bool.must.push({ match: { url: this.url } });
      }
      if (this.abstract !== "") {
        model.bool.must.push({ match: { abstract: this.abstract } });
      }
      this.$emit("update:modelValue", model);
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  justify-content: center;
}

.search-container {
  border-style: solid;
  border-radius: 10px;
  padding: 20px 30px;
  border-color: #2c3e50;
}

.grid-template-1 {
  display: grid;
  grid-template-columns: 100px 900px;
  justify-content: center;
  padding: 10px;
}

.grid-template-2 {
  display: grid;
  grid-template-columns: 100px 400px 100px 400px;
  justify-content: center;
  padding: 10px;
}

.grid-template-3 {
  display: grid;
  grid-template-columns: 100px 200px 200px 100px 400px;
  justify-content: center;
  padding: 10px;
}

button {
  background-color: #2c3e50;
  border: none;
  color: white;
  padding: 10px 30px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 15px 0;
  cursor: pointer;
  border-radius: 5px;
}

label {
  font-weight: bold;
  text-align: right;
  font-size: 16px;
  margin-right: 20px;
  padding: 5px 0;
}

input {
  border-radius: 5px;
  border-width: thin;
  height: 25px;
  font-size: 16px;
  padding: 2px 10px;
}
</style>
