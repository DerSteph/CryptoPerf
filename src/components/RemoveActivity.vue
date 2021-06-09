<template>
  <button class="btn btn-danger">{{ id }}</button>
</template>
<script>
import gql from "graphql-tag";

export default {
  apollo: {
    __schema: {
      query: gql`
        query {
          __schema {
            types {
              name
              enumValues {
                name
              }
            }
          }
        }
      `,
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
  props: ["id"],
};
</script>
