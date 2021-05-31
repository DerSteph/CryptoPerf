<template>
  <div id="chart">
    <div v-if="loading"></div>
    <div v-else>
      <h1>Overview</h1>
      <apexchart
        width="380"
        type="donut"
        :options="chartOptions"
        :series="series"
      ></apexchart>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";

export default {
  name: "Chart",
  data() {
    return {
      chartOptions: {
        labels: [],
      },
      series: [],
      loading: true,
    };
  },
  setup() {},
  async mounted() {
    this.loading = true;
    if (this.$route.params.uuid != null) {
      var allCoinAmountsByUUID = await this.$apollo.query({
        query: gql`
          query allCoinAmountsByUUID($uuid: String) {
            allCoinAmountsByUUID(uuid: $uuid) {
              cryptocurrency
              amount
              value
            }
          }
        `,
        variables: {
          uuid: this.$route.params.uuid,
        },
      });
      if (allCoinAmountsByUUID.data.allCoinAmountsByUUID == null) {
        this.$emit("unknownuuid", true);
      } else {
        allCoinAmountsByUUID.data.allCoinAmountsByUUID.forEach((caches) => {
          this.series.push(caches.amount);
          this.chartOptions.labels.push(caches.cryptocurrency);
          this.$emit("loaded");
          this.loading = false;
        });
      }
    }
  },
};
</script>
<style scoped>
</style>