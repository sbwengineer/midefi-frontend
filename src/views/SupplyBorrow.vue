<template>
  <div class="supply-borrow">
    <div v-if="dataLoaded">
      <div class="d-flex justify-center cir">
        <v-col class="d-flex justify-center cir-image">
          <v-progress-circular
            :rotate="270"
            :size="100"
            :width="15"
            :value="healthFactor"
            color="#536DFE"
          >
            <span class="cir-text">Health<br />{{ healthFactor }}%</span>
          </v-progress-circular>
        </v-col>
      </div>

      <v-row class="d-flex justify-cnter main">
        <v-col class="supply">
          <SupplyList />
        </v-col>
        <v-col class="borrow">
          <BorrowList />
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
import SupplyList from '@/components/supply/SupplyList.vue'
import BorrowList from '@/components/borrow/BorrowList.vue'
import { mapState, mapMutations } from 'vuex'
import * as constants from '@/store/constants'

export default {
  name: 'SupplyBorrow',
  components: {
    SupplyList,
    BorrowList,
  },
  data() {
    return {
      accountHealth: 1,
      currentComponent: null,
      hasEnteredToSomeMarket: true,
      transactionHash: null,
      dataLoaded: true,
    }
  },
  computed: {
    ...mapState({
      account: (state) => state.Session.account,
    }),
    healthFactor() {
      return this.accountHealth >= 1 ? 100 : (this.accountHealth * 100).toFixed(2)
    },
    healthFactorColor() {
      if (this.accountHealth <= 0.3) return '#EB5757'
      if (this.accountHealth > 0.3 && this.accountHealth <= 0.6) return '#F2994A'
      return '#24BD6B'
    },
    getHttpTokenBridge() {
      return process.env.VUE_APP_HTTP_TOKEN_BRIDGE
    },
  },
  mounted() {
    this.$on('reload', this.reset)
  },
  async created() {
    this.currentComponent = this.$route.params.tab === 'borrow' ? 'BorrowList' : 'SupplyList'
    this.accountHealth = await this.$middleware.getAccountHealth(this.account)
    this.hasEnteredToSomeMarket = await this.$middleware.hasEnteredToSomeMarket(this.account)
  },
  methods: {
    async reset() {
      this.accountHealth = await this.$middleware.getAccountHealth(this.account)
      this.hasEnteredToSomeMarket = await this.$middleware.hasEnteredToSomeMarket(this.account)
      //clear calculate apr
      this.setApr(0)
    },
    catchTx(obj) {
      const { promiseAction, symbol, nameAction, amount, isApprove } = obj
      this.transactionHash = null
      //validate obj has an action
      if (typeof promiseAction.then !== 'function') return
      //send snack
      this.setSnack('WAITING FOR CONFIRMATION')
      //await action tx (sender to wallet)
      promiseAction
        .then((transaction) => {
          this.transactionHash = transaction.hash
          //when approve tx send wait snack
          this.setWaitTxSnack()
          //await for wait tx
          return transaction.wait()
        })
        //TODO validate transactionResult
        // eslint-disable-next-line no-unused-vars
        .then((transactionResult) => {
          this.persistEventLocalStorage(nameAction, amount, this.transactionHash, Date.now(), true)

          if (isApprove === true)
            this.setSuccessApproveTxSnack({
              tx: this.transactionHash,
              token: symbol,
            })
          else
            this.setSuccessTxSnack({
              tx: this.transactionHash,
              token: symbol,
              amount: this.$options.filters.formatNumber(amount),
              action: nameAction,
            })
          this.reset()
        })
        .catch((error) => {
          const userError = typeof error === 'string' ? error : error.message || ''
          this.setFailTxSnack({ error: userError })
          this.persistEventLocalStorage(nameAction, amount, this.transactionHash, Date.now(), false)
        })
    },
    ...mapMutations({
      setSnack: constants.SNACK_SET,
      setSuccessTxSnack: constants.SNACK_SET_SUCCESS_TX,
      setSuccessApproveTxSnack: constants.SNACK_SET_SUCCESS_APPROVE_TX,
      setWaitTxSnack: constants.SNACK_SET_WAIT_TX,
      setFailTxSnack: constants.SNACK_SET_FAIL_TX,
      setCalulatedApr: constants.CALCULATE_APR,
    }),
    persistEventLocalStorage(event, amount, hash, date, status) {
      const txLS = this.$user.createTx(hash, event, amount, date, status)
      this.$user.addTxToAccountList(txLS, this.account)
    },
    setApr(calulateApr) {
      this.setCalulatedApr(calulateApr)
    },
  },
}
</script>
