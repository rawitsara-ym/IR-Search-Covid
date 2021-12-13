<template>
  <form @submit.prevent="callback">
    <input type="text" class="" v-model="value.match" /><button type="submit">
      <i class="fas fa-search"></i>
    </button>
  </form>
</template>
<script>
export default {
  mounted() {
    this.$emit("update:modelValue", { match: "" });
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
