<template>
  <v-container>
    <v-row justify="center">
      <v-col class="col-8">
        <v-card>
          <v-card-title>
            Orgs Created Per Day
          </v-card-title>
          <apexchart
            v-if="organisations.length > 0"
            type="line"
            :options="chartOptions"
            :series="countPerDay"
          />
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import moment from "moment";
import _ from "underscore";

export default {
  mounted() {
    this.fetchData();
  },
  data() {
    return {
      organisations: []
    };
  },
  methods: {
    async fetchData() {
      const query = `query {
                      organisations(take: 2000, sort: { createdAt: ASC }) {
                        nodes {
                          id
                          createdAt
                        }
                      }
                    }`;

      const rawResponse = await fetch("https://daolist.1hive.org", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ query })
      });

      const response = await rawResponse.json();

      this.organisations = response.data.organisations.nodes;
    }
  },
  computed: {
    orgsPerDay() {
      return _.groupBy(this.organisations, organisation =>
        moment(organisation.createdAt).format("MM-DD-YYYY")
      );
    },
    countPerDay() {
      // This could be simplified to just an array of values, but is keyed for potentially merging arrays.
      let countPerDay = {};
      Object.entries(this.orgsPerDay).forEach(([day, orgs]) => {
        countPerDay[day] = orgs.length;
      });
      return [{ name: "Created", data: Object.values(countPerDay) }];
    },
    days() {
      return Object.keys(this.orgsPerDay);
    },
    chartOptions() {
      return {
        xaxis: {
          categories: this.days,
          title: { text: "Date" }
        },
        yaxis: {
          title: { text: "Created" }
        }
      };
    }
  }
};
</script>

<style lang="scss">
.v-card {
  min-width: 275px;
}
</style>
