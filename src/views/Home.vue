<template>
  <div>
    <div v-if="loading" class="d-flex justify-content-center">
      <div class="spinner-border" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
      {{ loading }}
    </div>
    <div v-if="unknownUUID && !noUUID" class="d-flex justify-content-center">
      <div class="alert alert-danger" role="alert">unknown uuid</div>
    </div>
    <div class="container" v-else>
      <div class="row">
        <div class="col">
          <Prices @loaded="loaded" />
        </div>
        <div class="col">
          <Chart @unknownuuid="notKnown" @loaded="loaded" ref="chart" />
        </div>
      </div>
      <div class="row">
        <div class="col">
          <Activities @loaded="loaded" @reload="reload"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Prices from "@/components/graphql/Prices.vue";
import Chart from "@/components/graphql/Chart.vue";
import Activities from "@/components/graphql/Activities.vue";

export default {
  name: "Home",
  data() {
    return {
      unknownUUID: false,
      loading: true,
      noUUID: false,
    };
  },
  components: {
    Prices,
    Chart,
    Activities,
  },
  mounted() {
    this.loading = false;
  },
  created() {
    this.loading = true;
    if (this.$route.params.uuid == null) {
      this.noUUID = true;
    }
  },
  methods: {
    notKnown(value) {
      if (value == true) {
        this.unknownUUID = true;
        this.loading = false;
      }
    },
    loaded() {
      //console.log("Schalte aus");
      this.loading = false;
    },
    reload() {
      this.$refs.chart.reload();
    }
  },
};
</script>
