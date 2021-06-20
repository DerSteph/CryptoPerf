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
          <td>{{ coins.cryptocurrency }}</td>
          <td>{{ coins.amount }}</td>
          <td>{{ coins.value }}</td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
            <td colspan="2">Total:</td>
            <td>{{fullAmount}}</td>
        </tr>
      </tfoot>
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
          this.fullAmount = 0;
          data.allCoinAmountsByUUID.forEach(element => {
              this.fullAmount = this.fullAmount + element.value;
          });
      },
    },
  },
  data() {
    return {
      error: null,
      routeParam: this.$route.params.uuid,
      allCoinAmountsByUUID: [],
      fullAmount: 0,
    };
  },
};
</script>