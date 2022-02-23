<script >
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import HelloThree from "./components/HelloThree.vue";
import demo from "./components/demo.vue";
import demo2 from "./components/demo2.vue";
import demo3 from "./components/demo3.vue";
export default {
  components: {
    HelloThree,
    demo,
    demo2,
    demo3,
  },
  data() {
    return {
      currentTab: "HelloThree",
      tabs: ["HelloThree", "demo", "demo2", "demo3"],
    };
  },
};
</script>

<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <div class="header">
    <h1 id="title">three.js demo- {{ currentTab }}</h1>
    <button
      v-for="tab in tabs"
      :key="tab"
      :class="['tab-button', { active: currentTab === tab }]"
      @click="currentTab = tab"
    >
      {{ tab }}
    </button>
  </div>
  <component :is="currentTab" class="tab"></component>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
#title {
  top: 0;
  margin: 0 10px;
  display: inline-block;
}
.tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  background: #f0f0f0;
  margin-bottom: -1px;
  margin-right: -1px;
}
.tab-button:hover {
  background: #e0e0e0;
}
.tab-button.active {
  background: #e0e0e0;
}
</style>
