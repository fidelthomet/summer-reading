<template>
  <div id="app">
    <ScrollContainer msg="Hello Vue in CodeSandbox!"/>
    <div class="long">
      <ul>
        <li v-for="(p, i) in long" :key="`l-${i}`">
          <strong>{{p.title}}</strong> by
          <span>{{ p.authors }}</span>,
          <em>{{ p.venue }}</em>,
          <span>{{ p.year }}</span>
        </li>
      </ul>
    </div>
    <div class="footer">
      <h2>📚 More Info</h2>
      <img src="./assets/img/gruppenbild.png">
    </div>
  </div>
</template>

<script>
import ScrollContainer from "./components/ScrollContainer";
import long from "@/assets/data/long.json";
export default {
  name: "App",
  components: {
    ScrollContainer
  },
  data() {
    return {
      long
    };
  }
};
</script>

<style>
@import "./assets/css/base.css";
</style>
<style scoped>
#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#app > div {
  margin: 0 0 4rem;
}

.long,
.footer {
  width: 100%;
  max-width: 400px;
}
img {
  max-width: 100%;
}
</style>

