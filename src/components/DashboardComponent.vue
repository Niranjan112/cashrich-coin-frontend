<!-- eslint-disable prettier/prettier -->
<template>
  <v-container>
    <div>
      <v-card>
        <v-card-title
          >Welcome {{ user.firstName + " " + user.lastName }}
        </v-card-title>

        <v-card-text>
          <v-card-actions>
            <div>
              <v-btn color="success" @click="getCoins">Show Coins</v-btn>
            </div>
          </v-card-actions>

          <v-card class="mt-5">
            <v-card-text v-for="(item, key) in coinData" :key="key">
              <v-row>
                <v-col>
                  <h2>{{ item.name }}</h2>
                </v-col>
                <v-col>
                  <h3>{{ item.quote.USD.percent_change_24h }}</h3>
                </v-col>
                <v-col>
                  <h3>{{ item.symbol }}</h3>
                </v-col>
              </v-row>

              <v-row>
                <v-col>
                  <h3>Price: {{ item.quote.USD.price }}</h3>
                </v-col>
                <v-col>
                  <h3>Rank: {{ item.cmc_rank }}</h3>
                </v-col>
                <v-col> </v-col>
              </v-row>
            </v-card-text>
          </v-card>
        </v-card-text>
      </v-card>
    </div>
  </v-container>
</template>
<!-- eslint-disable prettier/prettier -->
<script>
export default {
  name: "DashboardCompnent",
  props: ["user"],
  data: () => ({
    coinData: null,
  }),

  methods: {
    async getCoins() {
      const response = await fetch(
        `http://localhost:8081/api/coin/${this.user.id}`,
        {
          method: "GET",
          headers: {
            "Content-Type": "application/json",
            "origin-api-key": "23232-2323fdefe-3343f",
          },
        }
      );

      const jsonData = await response.json();
      this.coinData = jsonData.data;
    },
  },
};
</script>
