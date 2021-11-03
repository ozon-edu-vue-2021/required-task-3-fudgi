<template>
  <div id="app">
    <div class="office">
      <Map :selected="selectedWorkPlace" @click="handleWorkPlaceSelect" />
      <SideMenu
        :person="selectedPerson"
        :isUserOpenned="isUserOpenned(selectedWorkPlace)"
        @update="handleWorkPlaceSelect"
      />
    </div>
  </div>
</template>

<script>
import Map from "./components/Map.vue";
import SideMenu from "./components/SideMenu.vue";
import people from "@/assets/data/people.json";

export default {
  name: "App",
  components: {
    Map,
    SideMenu,
  },
  data() {
    return {
      selectedWorkPlace: NaN,
    };
  },
  methods: {
    handleWorkPlaceSelect(id) {
      this.selectedWorkPlace = Number(id);
    },
    isUserOpenned(selected) {
      return !Number.isNaN(selected);
    },
  },
  computed: {
    selectedPerson() {
      if (Number.isNaN(this.selectedWorkPlace)) return;
      const personData = people.find(
        (item) => item.tableId === this.selectedWorkPlace
      );
      return personData;
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
  background: #e1e1e1;
  max-width: 1500px;
  margin: 0 auto;
}
</style>
