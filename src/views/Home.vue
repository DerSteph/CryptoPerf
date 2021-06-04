<template>
  <div>
    <div v-if="loading" class="d-flex justify-content-center">
      <div class="spinner-border" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>
    <div v-if="unknownUUID" class="d-flex justify-content-center">
      <div class="alert alert-danger" role="alert">
        unknown uuid
      </div>
    </div>
    <div class="container" v-else>
      <div class="row">
        <div class="col">
          <Prices @loaded="loaded" />
        </div>
        <div class="col">
          <Chart @unknownuuid="notKnown" @loaded="loaded" />
        </div>
      </div>
      <div class="row">
        <div class="col">
          <Activities @loaded="loaded" />
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
    };
  },
  components: {
    Prices,
    Chart,
    Activities,
  },
  mounted() {
    this.loading = true;
  },
  created() {
    this.loading = false;
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
  },
};
</script>
