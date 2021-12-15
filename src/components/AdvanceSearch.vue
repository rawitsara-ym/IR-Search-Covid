<template>
  <div class="container">
    <form @submit.prevent="callback">
      <div class="search-container">
        <div class="grid-template-2">
          <label for="title">Title</label>
          <input type="text" id="title_search" name="title" />
          <label for="author">Author</label>
          <input type="text" id="author_search" name="author" />
        </div>
        <div class="grid-template-3">
          <label for="published_date">Published</label>
          <input style="margin-right: 10px"
            type="date"
            id="published_date_from_search"
            name="published_date[from]"
          />
          <input style="margin-left: 10px"
            type="date"
            id="published_date_to_search"
            name="published_date[to]"
          />
          <label for="url">URL</label>
          <input type="text" id="url_search" name="url" />
        </div>
        <div class="grid-template-1">
          <label for="abstract">Abstract</label>
          <input type="text" id="abstract_search" name="abstract" />
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
