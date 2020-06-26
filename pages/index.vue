<template>

  <v-skeleton-loader :loading="loading" type="paragraph@3">


    <v-text-field v-model="query" label="Search country" clearable></v-text-field>


    <v-alert v-if="results.length === 0" type="error">
      No country found
    </v-alert>


    <v-card v-else>
      <v-list nav>
        <v-list-group v-for="(item, index) in results" :key="index" no-action>

          <template v-slot:activator>
            <v-list-item-content>
              <v-list-item-title v-text="item.Country"></v-list-item-title>
            </v-list-item-content>
          </template>


          <v-list-item>
            <v-list-item-content>
              <v-list-item-title>New Confirmed</v-list-item-title>
              <v-list-item-subtitle v-text="item.NewConfirmed"></v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item>
            <v-list-item-content>
              <v-list-item-title>Total Confirmed</v-list-item-title>
              <v-list-item-subtitle v-text="item.TotalConfirmed"></v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item>
            <v-list-item-content>
              <v-list-item-title>New Deaths</v-list-item-title>
              <v-list-item-subtitle v-text="item.NewDeaths"></v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item>
            <v-list-item-content>
              <v-list-item-title>Total Deaths</v-list-item-title>
              <v-list-item-subtitle v-text="item.TotalDeaths"></v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item>
            <v-list-item-content>
              <v-list-item-title>New Recovered</v-list-item-title>
              <v-list-item-subtitle v-text="item.NewRecovered"></v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item>
            <v-list-item-content>
              <v-list-item-title>Total Recovered</v-list-item-title>
              <v-list-item-subtitle v-text="item.TotalRecovered"></v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-list-item v-if="item.Date">
            <v-list-item-content>
              <v-list-item-title>Update Time</v-list-item-title>
              <v-list-item-subtitle v-text="item.Date"></v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

        </v-list-group>
      </v-list>
    </v-card>


  </v-skeleton-loader>

</template>

<script>
  export default {

    name: 'index',


    data() {
      return {
        loading: false,
        query: null,
        countries: [],
      }
    },


    created() {
      this.fetch();
    },


    computed: {
      results() {
        if (this.query) {
          return this.search();
        }
        return this.countries;
      },
    },


    methods: {

      fetch() {
        this.loading = true;
        this.$axios.get('summary')
          .then(response => {
            this.countries = response.data.Countries;

            const global = response.data.Global;
            global.Country = 'Global';
            this.countries.unshift(global)

          })
        .then(() => {
          this.loading = false;
        })
      },

      search() {
        return this.countries.filter(item => {
          return item.Country.toLowerCase().includes(this.query.toLowerCase());
        });
      },

    },

  }
</script>
