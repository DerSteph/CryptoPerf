<template>
  <div>
    <div v-if="$apollo.loading"></div>
    <div v-else-if="error"></div>
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
            v-for="activity in listAllActivitiesByUUID"
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
          <AddActivity />
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";
import AddActivity from "@/components/AddActivity.vue";

export default {
  components: {
    AddActivity,
  },
  apollo: {
    listAllActivitiesByUUID: {
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
      variables() {
        return {
          uuid: this.routeParam,
        };
      },
      error(error) {
        this.$emit("unknownuuid", true);
        console.error("test", error.message);
        this.error = error.message;
      },
      result({ data }) {
        if (data.allCoinAmountsByUUID == null) {
          this.$emit("unknownuuid", true);
          this.error == true;
        }
      },
    },
  },
  data() {
    return {
      filterType: "all",
      listAllActivitiesByUUID: [],
      error: null,
      routeParam: this.$route.params.uuid,
    };
  },
};
</script>
