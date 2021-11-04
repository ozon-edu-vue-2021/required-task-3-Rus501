<template>
  <div id="app">
    <div class="office">
      <Map @selected="getSelectedTable" @close="closeProfile" />
      <SideMenu
        @update:isUserOpenned="isUserOpenned = false"
        :isUserOpenned="isUserOpenned"
        :person="person"
        v-click-outside="closeProfile"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";

import people from "@/assets/data/people.json";

import ClickOutside from "vue-click-outside";
import "normalize.css";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  directives: {
    ClickOutside,
  },
  data() {
    return {
      person: {},
      isUserOpenned: false,
    };
  },
  methods: {
    getSelectedTable(tableId) {
      this.person = people.find((person) => person._id === tableId);
      setTimeout(() => {
        this.isUserOpenned = true;
      }, 0);
    },
    closeProfile() {
      this.isUserOpenned = false;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  background-color: #fafafa;
  padding: 24px;
  box-sizing: border-box;
}

html,
body,
#app {
  height: 100%;
}

* {
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

.office {
  display: grid;
  grid-template-columns: 1fr 320px;
  border-radius: 6px;
  border: 1px solid #ccd8e4;
  height: 100%;
  background: white;
  max-width: 1500px;
  margin: 0 auto;
  transition: margin 0.1s;
}
</style>
