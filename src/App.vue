<template>
  <div id="app">
    <div class="office">
      <Map :selected="selectedWorkPlace" @selectPlace="handleWorkPlaceSelect" />
      <SideMenu
        :person="selectedPerson"
        :isUserOpenned="isUserOpenned"
        @update="handleWorkPlaceSelect"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import people from "@/assets/data/people.json";
import checkIdSet from "./utils/checkIdSet";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data() {
    return {
      selectedWorkPlace: null,
    };
  },
  methods: {
    handleWorkPlaceSelect(id) {
      this.selectedWorkPlace = id ?? null;
    },
  },
  computed: {
    selectedPerson() {
      if (checkIdSet(this.selectedWorkPlace)) return;
      const personData = people.find(
        (item) => item.tableId === this.selectedWorkPlace
      );
      return personData;
    },
    isUserOpenned() {
      return !checkIdSet(this.selectedWorkPlace);
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

html {
  background-color: #e1e1e1;
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
  background: #f3f3f3;
  max-width: 1500px;
  margin: 0 auto;
}
</style>
