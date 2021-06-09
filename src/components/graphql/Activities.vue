<template>
  <div>
    <div v-if="$apollo.loading"></div>
    <div v-else-if="error"></div>
    <div v-else>
      <h1>Activities</h1>
      <table class="table">
        <thead>
          <tr>
            <th></th>
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
            <td><RemoveActivity v-bind:id="activity.id" /></td>
            <td>{{ activity.id }}</td>
            <td>{{ activity.activityType }}</td>
            <td>{{ activity.cryptocurrency }}</td>
            <td>{{ activity.amount }}</td>
            <td>{{ new Date(activity.date).toUTCString() }}</td>
          </tr>
          <AddActivity @reloadActivityAndGraph="reload" />
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";
import AddActivity from "@/components/AddActivity.vue";
import RemoveActivity from "@/components/RemoveActivity.vue";

export default {
  components: {
    AddActivity,
    RemoveActivity,
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
  methods: {
    async reload() {
      await this.$apollo.queries.listAllActivitiesByUUID.refetch();
      this.$emit("reload");
    }
  },
  computed: {
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
