<template>
  <v-container fluid>
    <v-row justify="center">
      <v-btn @click="updateChart">Update!</v-btn>
    </v-row>
    <v-row>
      <v-col class="col-xs-12 col-sm-4 col-md-3" v-for="n in 4" :key="n">
        <v-card style="min-width:176px">
          <div style="right: 0; position:absolute;" class="display-1 pa-3">
            +{{ series[0].data[2] }}%
          </div>
          <v-card-title style="position:absolute">Stat {{ n }}</v-card-title>
          <apexchart
            style="padding-top:30px;"
            type="line"
            :options="{
              ...chartOptions,
              chart: {
                toolbar: {
                  show: false
                }
              }
            }"
            :series="[series[0]]"
          >
          </apexchart>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-col class="col-xs-12 col-sm-6 col-md-4" v-for="n in 6" :key="n">
        <v-card class="graph">
          <v-card-title style="position:absolute">
            Multi-stat {{ n }}
          </v-card-title>
          <apexchart
            style="padding-top:20px;"
            type="area"
            :options="chartOptions"
            :series="series"
          />
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data: function() {
    return {
      chartOptions: {
        xaxis: {
          categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998]
        },
        theme: {
          monochrome: {
            enabled: true,
            color: "#255aee",
            shadeTo: "light",
            shadeIntensity: 0.65
          }
        }
      },
      series: [
        {
          name: "series-1",
          data: [30, 40, 45, 50, 49, 60, 70, 81]
        },
        {
          name: "series-2",
          data: [20, 30, 35, 40, 39, 50, 60, 71]
        }
      ]
    };
  },
  methods: {
    updateChart() {
      const max = 90;
      const min = 20;
      const newData = this.series[0].data.map(() => {
        return Math.floor(Math.random() * (max - min + 1)) + min;
      });

      this.series = [{ data: newData }, { data: this.series[0].data }];
    }
  }
};
</script>

<style lang="scss">
.v-card.graph {
  min-width: 275px;
}
</style>
