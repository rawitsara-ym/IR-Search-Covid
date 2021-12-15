<template>
  <form @submit.prevent="submit">
    <input type="text" class="" v-model="query" /><button type="submit">
      <i class="fas fa-search"></i>
    </button>
  </form>
</template>
<script>
export default {
  mounted() {
    this.$emit("update:modelValue", { query_string: { query: "" } });
  },
  props: ["modelValue"],
  emits: ["update:modelValue", "callback"],
  data() {
    return { query: "" };
  },
  methods: {
    submit() {
      this.$emit("callback");
    },
  },
  watch: {
    query(value) {
      this.$emit("update:modelValue", { query_string: { query: value } });
    },
  },
};
</script>

<style scoped>
form {
  display: flex;
  justify-content: center;
  height: 36px;
  margin: 10px 0;
}

input {
  border-radius: 5px 0 0 5px;
  border-width: thin;
  font-size: 18px;
  padding: 5px 10px;
  width: 500px;
}

button {
  background-color: #2c3e50;
  border: none;
  color: white;
  padding: 7px 20px;
  text-align: center;
  font-size: 18px;
  cursor: pointer;
  border-radius: 0 5px 5px 0;
}
</style>
