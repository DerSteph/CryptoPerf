<template>
  <div>
    <div v-if="loading" class="d-flex justify-content-center">
      <div class="spinner-border" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
      {{ loading }}
    </div>
    <div v-if="noUUID">
      <div class="px-4 py-5 my-5 text-center">
        <h1 class="display-5 fw-bold">CryptoPerf</h1>
        <div class="col-lg-6 mx-auto">
          <p class="lead mb-4">
            Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
            nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam
            erat, sed diam voluptua. At vero eos et accusam et justo duo dolores
            et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est
            Lorem ipsum dolor sit amet.
          </p>
          <div class="d-grid gap-2 d-sm-flex justify-content-sm-center">
            <div class="input-group mb-3">
              <input
                type="text"
                class="form-control btn-lg px-4 gap-3"
                placeholder="Enter UUID"
                v-model="uuid"
              />
              <button
                class="btn btn-primary btn-lg px-4"
                v-on:click="searchUUID"
              >
                Search
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <div v-if="unknownUUID" class="d-flex justify-content-center">
        <div class="alert alert-danger" role="alert">unknown uuid</div>
      </div>
      <div class="container" v-else>
        <div class="row">
          <div class="col">
            <PortfolioValue />
          </div>
          <div class="col">
            <Chart @unknownuuid="notKnown" @loaded="loaded" ref="chart" />
          </div>
        </div>
        <div class="row">
          <div class="col">
            <Prices @loaded="loaded" />
          </div>
          <div class="col">
            <Activities @loaded="loaded" @reload="reload" />
          </div>
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
import PortfolioValue from "@/components/graphql/PortfolioValue.vue";

export default {
  name: "Home",
  data() {
    return {
      unknownUUID: false,
      loading: true,
      noUUID: false,
      uuid: null,
    };
  },
  components: {
    Prices,
    Chart,
    Activities,
    PortfolioValue,
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
  watch: {
    $route() {
      if (this.$route.params.uuid == null) {
        this.noUUID = true;
      }
    },
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
    },
    searchUUID() {
      this.noUUID = false;
      this.$router.push({ name: "UUID", params: { uuid: this.uuid } });
    },
  },
};
</script>
