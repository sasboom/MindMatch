<template>
  <div>
    <Navbar />
    <b-spinner v-if="isLoading" label="Spinning"></b-spinner>
    <b-container v-else-if="matches && !isLoading" class="card-set">
      <b-card-group columns>
        <div v-for="match in matches" :key="match.id">
          <b-card :title="match.initiator.username" style="max-width: 20rem;" class="mb-4">
            <b-card-text>User introduction</b-card-text>
            <b-card-text>{{match.initiator.firstName}} {{match.initiator.lastName}}</b-card-text>
            <b-button-group vertical>
              <b-button
                @click="sendRequest(user)"
                variant="outline-success"
                class="actionBtns"
              >Accept</b-button>
              <b-button href="#" variant="outline-danger" class="actionBtns">Decline</b-button>
            </b-button-group>
            <template v-slot:footer>
              <small class="text-muted">Status: {{match.status}}</small>
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
import GET_MATCHES from "../graphql/GetMatches.gql";
import moment from 'moment';

export default {
  name: "MatchList",
  data() {
    return {
      isLoading: false,
      alertProps: {
        show: false,
        variant: "",
        message: ""
      },
      matches: []
    };
  },
  created: function() {
    this.isLoading = true;
    this.$apollo
      .query({
        query: GET_MATCHES
      })
      .then(response => {
        this.matches = response.data.matches.payload.map(match => ({
          ...match,
          requestedAt: moment(parseInt(match.createdAt)).fromNow(),
        }));
      });
  },
  mounted: function() {
    this.isLoading = false;
  },
  components: {
    Navbar,
  },
  methods: {
    resetAlertProps() {
      this.alertProps = {
        show: false,
        variant: "",
        message: ""
      };
    },
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
