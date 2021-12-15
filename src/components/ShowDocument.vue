<template>
  <div class="grid-container">
    <p style="font-weight: bold;">Title</p>
    <p>{{ doc.title }}</p>
  </div>
  <hr>
  <div class="grid-container">
    <p style="font-weight: bold;">Published</p>
    <p>{{ doc.published }}</p>
  </div>
  <hr>
  <div class="grid-container">
    <p style="font-weight: bold;">Authors</p>
    <p>{{ doc.authors.reduce((prev, curr) => `${prev}, ${curr}`) }}</p>
  </div>
  <hr>
  <div class="grid-container">
    <p style="font-weight: bold;">Abstract</p>
    <p>{{ doc.abstract }}</p>
  </div>
  <hr>
  <div class="grid-container">
    <p style="font-weight: bold;">URL</p>
    <p><a :href="doc.url" target="_blank">{{ doc.url }}</a></p>
  </div>
</template>

<script>
export default {
  name: 'ShowDocument',
  props: {
    doc: Object,
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.grid-container {
  display: grid;
  grid-template-columns: 150px auto;
  justify-content: flex-start;
  text-align: left;
}

</style>
