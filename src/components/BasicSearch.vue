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
<style scoped></style>
