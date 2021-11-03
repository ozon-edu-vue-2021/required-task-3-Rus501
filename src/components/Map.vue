<template>
  <div class="map">
    <h3>Карта офиса</h3>

    <div v-if="isLoading">Loading...</div>
    <div v-else class="map-root">
      <MapSVG ref="svg" />
      <TableSVG v-click-outside="closeProfile" v-show="false" ref="table" />
    </div>
  </div>
</template>

<script>
import * as d3 from "d3";
import ClickOutside from "vue-click-outside";

import MapSVG from "@/assets/images/map.svg";
import TableSVG from "@/assets/images/workPlace.svg";

import tables from "@/assets/data/tables.json";
import legend from "@/assets/data/legend.json";

export default {
  components: {
    MapSVG,
    TableSVG,
  },
  directives: {
    ClickOutside,
  },
  data() {
    return {
      isLoading: false,
      svg: null,
      g: null,
      tables,
      tableSVG: null,
    };
  },
  mounted() {
    this.popupItem = this.$el;

    this.svg = d3.select(this.$refs.svg);
    this.g = this.svg.select("g");
    this.tableSVGRef = d3.select(this.$refs.table);

    this.g ? this.drawTables() : console.error("error");
  },
  methods: {
    drawTables() {
      const svgTablesGroup = this.g.append("g").classed("groupPlaces", true);

      this.tables.map((table) => {
        const svgTable = svgTablesGroup
          .append("g")
          .attr("transform", `translate(${table.x}, ${table.y}) scale(0.5)`)
          .attr("id", table._id)
          .classed("employer-table", true);

        svgTable
          .append("g")
          .attr("transform", `rotate(${table.rotate || 0})`)
          .attr("group_id", table.group_id)
          .html(this.tableSVGRef.html())
          .attr(
            "fill",
            legend.find((it) => it.group_id === table.group_id)?.color ??
              "transparent"
          )
          .on("click", () => {
            this.$emit("selected", table._id);
          });
      });
    },
    closeProfile(event) {
      console.log(event.target);
      this.$emit("close");
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
