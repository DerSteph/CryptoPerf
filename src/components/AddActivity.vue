<template>
  <tr>
    <td>
      <button class="btn btn-primary" v-on:click="addActivity">new</button>
    </td>
    <td></td>
    <td>
      <select class="form-select" v-model="result.activityType">
        <option
          :value="activityType.name"
          v-for="(activityType, index) in ActivityTypes"
          :key="index"
        >
          {{ activityType.name }}
        </option>
      </select>
    </td>
    <td>
      <select class="form-select" v-model="result.cryptocurrency">
        <option
          :value="cryptocurrency.name"
          v-for="(cryptocurrency, index) in Cryptocurrencies"
          :key="index"
        >
          {{ cryptocurrency.name }}
        </option>
      </select>
    </td>
    <td>
      <input class="form-control" v-model="result.amount" />
    </td>
    <td>
      <input type="date" class="form-control" v-model="result.date" />
      <input type="time" class="form-control" v-model="result.time" />
    </td>
  </tr>
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
  data() {
    return {
      filterType: "all",
      Cryptocurrencies: [],
      ActivityTypes: [],
      error: null,
      result: {
        activityType: null,
        cryptocurrency: null,
        amount: null,
        date: null,
        time: null,
      },
    };
  },
  methods: {
    async addActivity() {
      console.log(this.result);
      //const result = 
      await this.$apollo.mutate({
        // Query
        mutation: gql`
          mutation (
            $uuid: String!
            $type: ActivityType!
            $cryptocurrency: Cryptocurrency!
            $amount: Float!
            $date: String!
          ) {
            addActivity(
              uuid: $uuid
              type: $type
              cryptocurrency: $cryptocurrency
              amount: $amount
              date: $date
            ) {
              id
            }
          }
        `,
        // Parameters
        variables: {
          uuid: this.$route.params.uuid,
          type: this.result.activityType,
          cryptocurrency: this.result.cryptocurrency,
          amount: parseFloat(this.result.amount),
          date: this.result.date + " " + this.result.time,
        },
      });
      this.$emit("reloadActivityAndGraph", true);
    },
  },
};
</script>
