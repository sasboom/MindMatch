<template>
  <div>
    <Navbar />
    <MessageAlert
      :show="alertProps.show"
      :variant="alertProps.variant"
      :message="alertProps.message"
      :dismissedFn="resetAlertProps"
    />
    <b-spinner v-if="isLoading" label="Spinning"></b-spinner>
    <b-container v-else-if="randomUsers && !isLoading" class="card-set">
      <b-card-group columns>
        <div v-for="user in randomUsers" :key="user.id">
          <b-card :title="user.username" style="max-width: 20rem;" class="mb-4">
            <b-card-text>User introduction</b-card-text>
            <b-card-text>{{user.firstName}} {{user.lastName}}</b-card-text>
            <b-button-group vertical>
              <b-button
                @click="sendRequest(user)"
                variant="outline-success"
                class="actionBtns"
              >Send Request</b-button>
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
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
import MessageAlert from "../components/MessageAlert";
import MATCH_REQUEST from "../graphql/MatchRequest.gql";
import RANDOM_USERS_QUERY from "../graphql/GetRandomUsers.gql";

export default {
  name: "MatchFinder",
  data() {
    return {
      isLoading: false,
      alertProps: {
        show: false,
        variant: "",
        message: ""
      },
      randomUsers: []
    };
  },
  created: function() {
    this.isLoading = true;
    this.$apollo
      .query({
        query: RANDOM_USERS_QUERY
      })
      .then(response => {
        this.randomUsers = response.data.randomUsers.payload;
      });
  },
  mounted: function() {
    this.isLoading = false;
  },
  components: {
    Navbar,
    MessageAlert
  },
  methods: {
    resetAlertProps() {
      this.alertProps = {
        show: false,
        variant: "",
        message: ""
      };
    },
    sendRequest(user) {
      return this.$apollo
        .mutate({
          mutation: MATCH_REQUEST,
          variables: {
            input: {
              initiatorUserId: "6e3ab5ee-80aa-44ef-9395-8db9fe761c29",
              requestedUserId: user.id
            }
          },
          update: () => {
            this.randomUsers = this.randomUsers.filter(i => i.id !== user.id);
            this.alertProps = {
              show: true,
              variant: "success",
              message: `A match request has been sent out to ${user.firstName} ${user.lastName} AKA ${user.username}.`
            };
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
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
