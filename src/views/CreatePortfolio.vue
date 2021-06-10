<template>
  <div>
    <h1>Create Portfolio</h1>
    <button class="btn btn-primary" v-on:click="createPortfolio">Create</button>
    <input class="form-control" readonly v-if="uuid" :value="uuid">
  </div>
</template>
<script>
import gql from "graphql-tag";

export default {
  methods: {
    async createPortfolio() {
      var query = await this.$apollo.mutate({
        // Query
        mutation: gql`
          mutation {
            createPortfolio {
              uuid
            }
          }
        `,
        // Parameters
        variables: {
        },
      });
    this.uuid = query.data.createPortfolio.uuid;
    },
  },
  data() {
      return {
          uuid: null,
      }
  }
};
</script>