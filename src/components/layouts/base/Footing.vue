<template>
  <div>
    <v-footer class="footer" color="lighten-1" padless>
      <v-container fluid class="black-footer">
        <v-container>
          <v-row justify="center" class="black-footer">
            <v-col lg="4" sm="4" cols="12" class="pl-5 state">
              <h1>Company</h1>
              <a class="mt-3" href="docs/introduction"> About us </a>
              <a href=""> Contact us </a>
              <a href="privacy"> Privacy Policy </a>
              <a href=""> Terms &#38; Conditions </a>
            </v-col>
            <v-col lg="4" sm="4" cols="12" class="pl-5 state">
              <h1>Guides</h1>
              <a href=""> Documentation </a>
              <a href=""> FAQ </a>
            </v-col>
            <v-col lg="4" sm="4" cols="12" class="pl-5 state">
              <h1>Contact</h1>
              <a class="mt-3" href=""> Address </a>
            </v-col>
          </v-row>
        </v-container>

        <v-row class="copyright-text">
          <div cols="5">
            <div class="copyright text-1 state">
              &#169;2022 Market Indicator. All rights reserved.
            </div>
          </div>
          <div class="text-right">
            <div class="copyright text-1">Follow us on social</div>
          </div>
          <div class="copyright-image">
            <a href="https://t.me/marketindicatorapp" target="_blank">
              <img src="../../../assets/footing/telegram.png" width="30px" height="30px" />
            </a>
          </div>
          <div class="copyright-image">
            <a href="https://twitter.com/marketindicato2" target="_blank">
              <img src="../../../assets/footing/Medium.png" width="30px" height="30px" />
            </a>
          </div>
        </v-row>
      </v-container>
    </v-footer>
  </div>
</template>

<script>
import { COMMIT_HASH } from '@/config/constants'

export default {
  name: 'Footing',
  data() {
    return {
      commitHash: COMMIT_HASH,
    }
  },
}
</script>
