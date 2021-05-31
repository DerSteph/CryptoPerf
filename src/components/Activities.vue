<template>
  <div>
    <div v-if="loading">
    </div>
    <div v-else>
      <h1>Activities</h1>
      <table class="table">
        <thead>
          <tr>
            <th>ID</th>
            <th>Activity Type</th>
            <th>Cryptocurrency</th>
            <th>Amount</th>
            <th>Date</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="activity in data"
            :key="activity.id"
            :class="{
              'table-success': activity.activityType == 'add',
              'table-danger': activity.activityType == 'remove',
            }"
          >
            <td>{{ activity.id }}</td>
            <td>{{ activity.activityType }}</td>
            <td>{{ activity.cryptocurrency }}</td>
            <td>{{ activity.amount }}</td>
            <td>{{ activity.date }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";

export default {
  name: "Activies",
  data() {
    return {
      data: [],
      loading: true,
    };
  },
  async mounted() {
    this.loading = true;
    if (this.$route.params.uuid != null) {
      var listAllActivitiesByUUID = await this.$apollo.query({
        query: gql`
          query listAllActivitiesByUUID($uuid: String!) {
            listAllActivitiesByUUID(uuid: $uuid) {
              cryptocurrency
              amount
              date
              activityType
              id
            }
          }
        `,
        variables: {
          uuid: this.$route.params.uuid,
        },
      });
      if (listAllActivitiesByUUID.data.listAllActivitiesByUUID == null) {
        this.$emit("unknownuuid", true);
      } else {
        this.data = listAllActivitiesByUUID.data.listAllActivitiesByUUID;
        this.$emit("loaded");
        this.loading = false;
      }
    }
  },
};
</script>
<style scoped>
</style>
