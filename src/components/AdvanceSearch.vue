<template>
  <form @submit.prevent="submit">
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
    <label for="abstract">Abstract</label>
    <input
      type="text"
      id="abstract_search"
      name="abstract"
      v-model="abstract"
      @input="update_query()"
    />
    <button type="submit">Search <i class="fas fa-search"></i></button>
  </form>
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
<style scoped></style>
