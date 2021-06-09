<template>
  <div id="chart">
    <div v-if="$apollo.loading"></div>
    <div v-else-if="error"></div>
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
  apollo: {
    allCoinAmountsByUUID: {
      query: gql`
        query allCoinAmountsByUUID($uuid: String) {
          allCoinAmountsByUUID(uuid: $uuid) {
            cryptocurrency
            amount
            value
          }
        }
      `,
      variables() {
        return {
          uuid: this.routeParam,
        };
      },
      error(error) {
        this.$emit("unknownuuid", true);
        console.error("test", error.message);
        this.error = error.message;
      },
      result({ data }) {
        if (this.series.length > 0 || this.chartOptions.labels > 0) {
          this.series = [];
          this.chartOptions.labels = [];
        }
        if (data.allCoinAmountsByUUID == null) {
          this.$emit("unknownuuid", true);
          this.error == true;
        } else {
          data.allCoinAmountsByUUID.forEach((caches) => {
            this.series.push(caches.amount);
            this.chartOptions.labels.push(caches.cryptocurrency);
            this.$emit("loaded");
            this.loading = false;
          });
        }
        this.loading = false;
      },
    },
  },
  data() {
    return {
      filterType: "all",
      allCoinAmountsByUUID: [],
      error: null,
      chartOptions: {
        labels: [],
      },
      series: [],
      routeParam: this.$route.params.uuid,
    };
  },
  methods: {
    async reload() {
      await this.$apollo.queries.allCoinAmountsByUUID.refetch();
    },
  },
};
</script>
