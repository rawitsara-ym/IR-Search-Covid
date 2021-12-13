<template>
  <form @submit.prevent="callback">
    <label for="title">Title</label>
    <input type="text" id="title_search" name="title" />
    <label for="author">Author</label>
    <input type="text" id="author_search" name="author" />
    <label for="published_date">Published</label>
    <input
      type="date"
      id="published_date_from_search"
      name="published_date[from]"
    />
    <input
      type="date"
      id="published_date_to_search"
      name="published_date[to]"
    />
    <label for="url">URL</label>
    <input type="text" id="url_search" name="url" />
    <label for="abstract">Abstract</label>
    <input type="text" id="abstract_search" name="abstract" />
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
  methods: {
    submit() {
      this.$emit("callback");
    },
  },
  computed: {
    value: {
      get() {
        return this.modelValue;
      },
      set(value) {
        this.$emit("update:modelValue", { match: value });
      },
    },
  },
};
</script>
<style scoped></style>
