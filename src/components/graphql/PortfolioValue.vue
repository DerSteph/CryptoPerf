<template>
  <div>
      <h1>Value</h1>
    <table class="table">
      <thead>
        <tr>
          <th>Cryptocurrency</th>
          <th>Amount</th>
          <th>Value</th>
        </tr>
      </thead>
      <tbody>
          <tr v-for="(coins, index) in allCoinAmountsByUUID" :key="index">
            <td>{{coins.cryptocurrency}}</td>
            <td>{{coins.amount}}</td>
            <td>{{coins.value}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import gql from "graphql-tag";

export default {
  apollo: {
    allCoinAmountsByUUID: {
      query: gql`
        query ($uuid: String!) {
          allCoinAmountsByUUID(uuid: $uuid) {
            amount
            cryptocurrency
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
        this.error = error.message;
      },
      result({ data }) {
        this.Cryptocurrencies = data.__schema.types.find(
          (o) => o.name === "Cryptocurrency"
        ).enumValues;
        this.ActivityTypes = data.__schema.types.find(
          (o) => o.name === "ActivityType"
        ).enumValues;
      },
    },
  },
      data() {
      return {
        error: null,
        routeParam: this.$route.params.uuid,
        allCoinAmountsByUUID: [],
      };
    },
};
</script>