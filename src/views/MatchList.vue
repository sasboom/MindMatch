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
    <b-container v-else-if="matches.length > 0 && !isLoading" class="card-set">
      <b-card-group columns>
        <div v-for="match in matches" :key="match.id">
          <b-card :title="match.initiator.username" style="max-width: 20rem;" class="mb-4">
            <b-card-text>{{match.initiator.firstName}} {{match.initiator.lastName}}</b-card-text>
            <b-button-group vertical>
              <b-button
                @click="sendRequest(match, 'APPROVED')"
                variant="outline-success"
                class="actionBtns"
                v-if="match.status === 'PENDING'"
              >Accept</b-button>
              <b-button
                @click="sendRequest(match, 'REJECTED')"
                variant="outline-danger"
                class="actionBtns"
                v-if="match.status === 'PENDING'"
              >Decline</b-button>
              <b-button
                @click="startChat(match)"
                variant="success"
                class="actionBtns"
                v-if="match.status === 'APPROVED'"
              >Start Chat </b-button>
            </b-button-group>
            <template v-slot:footer>
              <small class="text-muted">Status:<b-card-text v-bind:class="{'status-text-approved': match.status === 'APPROVED', 'status-text-rejected': match.status === 'REJECTED'}">{{match.status}}</b-card-text></small>
            </template>
          </b-card>
        </div>
      </b-card-group>
    </b-container>
    <!-- No result -->
    <div v-else-if="matches.length <= 0">No result :(</div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
import MessageAlert from '../components/MessageAlert';
import GET_MATCHES from "../graphql/GetMatches.gql";
import UPDATE_MATCH from "../graphql/MatchUpdate.gql";
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
    sendRequest({id}, status) {
      return this.$apollo
        .mutate({
          mutation: UPDATE_MATCH,
          variables: {
            input: {
              id: id,
              status: status
            }
          },
          update: () => {
            this.matches.map(i => {
              if (i.id === id) {
                i.status = status;
              }
            });
            this.alertProps = {
              show: true,
              variant: "success",
              message: `The match has been approved.`
            };
          }
        })
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
.status-text-approved {
  font-weight: bold;
  color: green;
}
.status-text-rejected {
  font-weight: bold;
  color: red;
}

</style>
