<template>
  <ApolloQuery :query="require('../graphql/GetRandomUsers.gql')">
    <template v-slot="{ result: { loading, error, data } }">
      <!-- Loading -->
      <div v-if="loading">Loading...</div>

      <!-- Error -->
      <div v-else-if="error">An error occurred</div>

      <!-- Result -->
      <b-container v-else-if="data" class="card-set">
        <b-card-group deck>
          <div v-for="user in data.randomUsers.payload" :key="user.id">
            <b-card
              :title="user.username"
              img-src="https://picsum.photos/600/300/?image=25"
              img-alt="Image"
              img-top
              style="max-width: 20rem;"
              class="mb-2"
            >
              <b-card-text>User introduction</b-card-text>
              <b-card-text>{{user.firstName}} {{user.lastName}}</b-card-text>
              <b-button-group vertical>
                <b-button href="#" variant="outline-success" class="actionBtns">Send Request</b-button>
                <b-button href="#" variant="outline-danger" class="actionBtns">Decline</b-button>
              </b-button-group>
            </b-card>
          </div>
        </b-card-group>
      </b-container>

      <!-- No result -->
      <div v-else>No result :(</div>
    </template>
  </ApolloQuery>
</template>

<script>
export default {
  name: "MatchFinder"
};
</script>

<style scoped>
.actionBtns {
  margin-bottom: 1vh;
}
.card-set {
  margin-top: 2vh;
}
</style>
