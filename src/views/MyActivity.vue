<template>
  <div class="my-activity">
    <div v-if="dataLoaded" class="upper-banner">
      <v-card class="graphics-card d-flex align-center card-1" width="1114px" height="151px">
        <v-col cols="4" class="d-flex supp">
          <img v-img :src="require('@/assets/myActivity/1.png')" />
          <p class="supp-1">Supplied</p>
          <h1 class="supp-2">$198K</h1>
        </v-col>
        <v-col cols="4" class="d-flex justify-center border call">
          <img v-img :src="require('@/assets/myActivity/2.png')" />
          <p class="call-1">Callateral</p>
          <h1 class="call-2">$2.4K</h1>
        </v-col>
        <v-col cols="4" class="d-flex justify-right borr">
          <img v-img :src="require('@/assets/myActivity/3.png')" />
          <p class="borr-1">Borrowed</p>
          <h1 class="borr-2">$89K</h1>
        </v-col>
      </v-card>
      <!-- halth factor -->
      <v-row>
        <v-col cols="3">
          <v-card class="graphics-card d-flex align-center health" width="304px" height="378px">
            <v-row class="d-flex align-center text-health">
              <h2>Health Factor</h2>
            </v-row>
            <v-row class="circular">
              <v-progress-circular
                :rotate="270"
                :size="181"
                :width="25"
                :value="healthFactor"
                color="#536DFE"
              >
                {{ accountHealth }}%
              </v-progress-circular>
            </v-row>
          </v-card>
        </v-col>
        <v-col class="pa-0 history">
          <v-card class="graphics-card container" width="737px" height="368px">
            <TransactionList />
          </v-card>
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
// import SupplyBorrowGraph from '@/components/dashboard/SupplyBorrowGraph.vue'
import TransactionList from '@/components/dashboard/TransactionList.vue'
import { mapState } from 'vuex'
import { cTokensDetails } from '../middleware/constants'
import Vue from 'vue'
import VueNumber from 'vue-number-animation'

Vue.use(VueNumber)

export default {
  name: 'MyActivity',
  components: {
    // SupplyBorrowGraph,
    TransactionList,
  },
  data() {
    return {
      healthFactor: 0,
      totalBalance: [],
      totalSupplied: [],
      totalBorrowed: [],
      showHealthWarning: null,
      polling: null,
      markets: [],
      accountStorage: '',
      dataLoaded: true,
    }
  },
  computed: {
    ...mapState({
      account: (state) => state.Session.account,
    }),
    accountHealth() {
      return this.healthFactor.toFixed(2)
    },
    healthColor() {
      if (this.risk === 'high') return '#EB5757'
      if (this.risk === 'medium') return '#F2994A'
      return '#24BD6B'
    },
    risk() {
      if (this.healthFactor >= 30) {
        if (this.healthFactor >= 60) {
          return 'low'
        }
        return 'medium'
      }
      return 'high'
    },
  },
  beforeDestroy() {
    clearInterval(this.polling)
  },
  async created() {
    for (const market of cTokensDetails) {
      this.$middleware.getAdapterPrice(market.adapter).then((price) => {
        this.markets.push({
          symbol: market.underlying.symbol,
          logo: market.logo,
          adapter: market.adapter,
          price: price.toNumber(),
          oldPrice: price.toNumber(),
          priceUp: 0,
        })
      })
    }
    await this.fetchData()
    this.dataLoaded = true
    this.pollData()
  },
  methods: {
    pollData() {
      this.polling = setInterval(async () => {
        await this.fetchData()
      }, 20000)
    },
    async fetchData() {
      const { borrowValue, supplyValue } = await this.$middleware.getTotalSupplysAndBorrows(
        this.account,
      )
      //refact this totals for int var and set in last hook
      this.totalBorrowed[0] = borrowValue.toNumber()
      this.totalSupplied[0] = supplyValue.toNumber()
      this.totalBalance[0] = supplyValue.minus(borrowValue).toNumber()
      const health = await this.$middleware.getAccountHealth(this.account)
      this.healthFactor = health > 1 ? 100 : health * 100
      this.showHealthWarning = Number(this.healthFactor) === 0
      this.getMarketsPrices()
    },
    async getMarketsPrices() {
      if (this.markets.length < 1) return
      for (const indice in this.markets) {
        this.$middleware.getAdapterPrice(this.markets[indice].adapter).then((price) => {
          this.markets[indice].price = price.toNumber()
          this.markets[indice].priceUp = this.markets[indice].price - this.markets[indice].oldPrice
          this.markets[indice].oldPrice = this.markets[indice].price
        })
      }
    },
    tweenedFormat(number) {
      return number.toFixed(3)
    },
  },
}
</script>
