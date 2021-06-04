<template>
  <tr>
    <td><button class="btn btn-primary">new</button></td>
    <td>
      <select class="form-select">
        <option selected value="add">add</option>
        <option value="remove">remove</option>
      </select>
    </td>
    <td>
      <select class="form-select">
        <option
          :value="cryptocurrency.name"
          v-for="(cryptocurrency, index) in Cryptocurrencies"
          :key="index"
        >
          {{ cryptocurrency.name }}
        </option>
      </select>
    </td>
  </tr>
</template>

<script>
import gql from "graphql-tag";

export default {
  apollo: {
    __type: {
      query: gql`
        query Cryptocurrencies {
          __type(name: "Cryptocurrency") {
            enumValues {
              name
            }
          }
        }
      `,
      result({ data }) {
        this.Cryptocurrencies = data.__type.enumValues;
      },
    },
    __type2: {
      query: gql`
        query ActivityType {
          __type(name: "ActivityType") {
            enumValues {
              name
            }
          }
        }
      `,
      result({ data }) {
        this.Cryptocurrencies = data.__type.enumValues;
      },
    },
  },
  data() {
    return {
      filterType: "all",
      Cryptocurrencies: [],
      ActivityTypes: [],
      error: null,
    };
  },
};
</script>
