<template>
  <div class="map" @click="clickHandler">
    <h3>Карта офиса</h3>

    <div v-if="!isLoading" class="map-root">
      <MapSVG ref="svg" />
      <TableSVG v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import * as d3 from "d3";
import MapSVG from "@/assets/images/map.svg";
import TableSVG from "@/assets/images/workPlace.svg";
import tables from "@/assets/data/tables.json";
import legend from "@/assets/data/legend.json";

export default {
  components: {
    MapSVG,
    TableSVG,
  },
  data() {
    return {
      isLoading: false,
      svg: null,
      g: null,
      tables: [],
      tableSVG: null,
    };
  },
  props: {
    selected: {
      type: Number,
      default: () => NaN,
    },
  },
  mounted() {
    this.svg = d3.select(this.$refs.svg);
    this.g = this.svg.select("g");
    this.tableSVG = d3.select(this.$refs.table);

    this.tables = tables;

    if (this.g) {
      this.drawTables();
    } else console.log("ERROR");
  },
  methods: {
    drawTables() {
      const svgTablesGroup = this.g.append("g").classed("groupPlaced", true);
      this.tables.map(({ _id, x, y, rotate, group_id }) => {
        const transformStyle = `translate(${x}, ${y}) scale(0.5) rotate(${
          rotate || 0
        })`;
        const fillColor =
          legend.find((it) => it.group_id === group_id)?.color ?? "transparent";

        svgTablesGroup
          .append("g")
          .attr("transform", transformStyle)
          .attr("data-id", _id)
          .html(this.tableSVG.html())
          .attr("fill", fillColor);
      });
    },
    clickHandler(e) {
      const id = e.target.closest("[data-id]")?.dataset?.id;
      this.$emit("selectPlace", id);
    },
  },
  watch: {
    selected: function (newVal) {
      this.g.select(".selected-place").classed("selected-place", false);
      if (newVal)
        this.g.select(`[data-id='${newVal}']`).classed("selected-place", true);
    },
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>

<style>
.selected-place {
  filter: drop-shadow(0px 0px 3px #677eff);
}
</style>
