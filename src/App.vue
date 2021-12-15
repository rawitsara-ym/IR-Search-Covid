<template>
  <div class='title'>
    <img class='icon flip' alt="covid-19 logo" src="./assets/covid_icon.png">
    <h1>COVID-19 Research</h1>
    <img class='icon' alt="covid-19 logo" src="./assets/covid_icon.png">
  </div>
  <!-- <ShowDocument v-bind:doc="doc"/> -->
  <search></search>
</template>

<script>
import Search from './components/Search.vue'
// import ShowDocument from './components/ShowDocument.vue'

export default {
  data() {
    return { doc: {
                    title: "COVID-19 virtual patient cohort reveals immune mechanisms driving disease outcomes", 
                    published: '01-01-2020', 
                    authors: ["Rutu Karia", "Ishita Gupta", "Harshwardhan Khandait", "Ashima Yadav", "Anmol Yadav"],
                    abtract: 'To understand the diversity of immune responses to SARS-CoV-2 and distinguish features that 39 predispose individuals to severe COVID-19  we developed a mechanistic  within-host mathematical 40 model and virtual patient cohort. Our results indicate that virtual patients with low production rates of 41 infected cell derived IFN subsequently experienced highly',
                    url: 'https://journals.plos.org/plospathogens/article?id=10.1371/journal.ppat.1009753'
                    }
            }
  },
  name: 'App',
  components: {
    Search
    // ShowDocument
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #2c3e50; */
  margin: 20px 60px 0px 60px;
}
.title {
  font-size: large;
  display: flex;
  justify-content: center;
}
.icon {
  width: 60px;
  height: 60px;
  padding: 20px;
}
.flip {
  transform: scaleX(-1);
}
</style>
