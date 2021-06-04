<template>
  <div>
    <div v-if="$apollo.loading"></div>
    <div v-else-if="error">
      {{ error }}
    </div>
    <div v-else-if="$apollo.data">
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
          <tr v-for="price in allPrices" :key="price.cryptocurrency">
            <td>{{ price.cryptocurrency }}</td>
            <td>{{ price.fiatcurrency }}</td>
            <td>{{ price.price }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div v-else>No data</div>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  apollo: {
    allPrices: {
      query: gql`
        query {
          allPrices(fiatcurrency: EUR) {
            cryptocurrency
            fiatcurrency
            price
          }
        }
      `,
      error(error) {
        console.error("test", error.message);
        this.error = error.message;
      },
    },
  },
  data() {
      return {
          filterType: "all",
          allPrice: [],
          error: null,
      }
  },
  methods: {
    test1() {
      console.log(this.$apollo);
    },
  },
};
</script>
