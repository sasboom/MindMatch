<template>
  <div>
    <Navbar />
    <ApolloQuery :query="require('../graphql/GetRandomUsers.gql')">
      <template v-slot="{ result: { loading, error, data } }">
        <!-- Loading -->
        <div v-if="loading">Loading...</div>

        <!-- Error -->
        <div v-else-if="error">An error occurred</div>

        <!-- Result -->
        <b-spinner v-if="isLoading" label="Spinning"></b-spinner>
        <b-container v-else-if="data && !isLoading" class="card-set">
          <b-card-group columns>
            <div v-for="user in data.randomUsers.payload" :key="user.id">
              <b-card :title="user.username" style="max-width: 20rem;" class="mb-4">
                <b-card-text>User introduction</b-card-text>
                <b-card-text>{{user.firstName}} {{user.lastName}}</b-card-text>
                <b-button-group vertical>
                  <b-button href="#" variant="outline-success" class="actionBtns">Send Request</b-button>
                  <b-button href="#" variant="outline-danger" class="actionBtns">Decline</b-button>
                </b-button-group>
                <template v-slot:footer>
                  <small class="text-muted">Last updated 3 mins ago</small>
                </template>
              </b-card>
            </div>
          </b-card-group>
        </b-container>

        <!-- No result -->
        <div v-else>No result :(</div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
export default {
  name: "MatchFinder",
  data() {
    return {
      isLoading: false
    };
  },
  created: function() {
    this.isLoading = true;
  },
  mounted: function() {
    this.isLoading = false;
  },
  components: {
    Navbar
  }
};
</script>

<style scoped>
.actionBtns {
  margin-bottom: 1vh;
}
.card-set {
  margin-top: 4vh;
}
</style>
