<template>
  <div>
    <div v-if="loading"></div>
    <div v-else>
      <h1>Prices</h1>
      <table class="table">
        <thead>
          <tr>
            <th>Cryptocurrency</th>
            <th>Fiatcurrency</th>
            <th>Price</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="price in data" :key="price.cryptocurrency">
            <td>{{ price.cryptocurrency }}</td>
            <td>{{ price.fiatcurrency }}</td>
            <td>{{ price.price }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "Prices",
  async mounted() {
    this.loading = true;
    var allPrices = await this.$apollo.query({
      query: gql`
        query {
          allPrices(fiatcurrency: EUR) {
            cryptocurrency
            fiatcurrency
            price
          }
        }
      `,
      variables: {
        uuid: this.$route.params.uuid,
      },
    });
    this.data = allPrices.data.allPrices;
    this.$emit("loaded");
    this.loading = false;
  },
  data() {
    return {
      data: [],
      loading: true,
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
