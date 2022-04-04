<template>
  <div>
    <v-footer class="footer" color="lighten-1" padless>
      <v-container fluid class="black-footer">
        <v-container>
          <v-row justify="center" class="black-footer">
            <v-col lg="4" sm="4" cols="12" class="pl-5">
              <h1>Company</h1>
              <a class="mt-3" href="docs/introduction"> About us </a>
              <a href=""> Contact us </a>
              <a href="privacy"> Privacy Policy </a>
              <a href=""> Terms &#38; Conditions </a>
            </v-col>
            <v-col lg="4" sm="4" cols="12" class="pl-5">
              <h1>Guides</h1>
              <a href=""> Documentation </a>
              <a href=""> FAQ </a>
            </v-col>
            <v-col lg="4" sm="4" cols="12" class="pl-5">
              <h1>Contact Us</h1>
              <a class="mt-3" href=""> Address </a>
            </v-col>
          </v-row>
        </v-container>

        <v-row class="copyright-text">
          <v-col cols="5">
            <div class="copyright text-left text-1">
              &#169;2022 Market Indicator. All rights reserved.
            </div>
          </v-col>
          <v-col cols="2" class="text-right pr-16">
            <div class="copyright text-left text-1">Follow us on social</div>
          </v-col>
          <v-col cols="1" class="copyright-image">
            <a href="https://t.me/marketindicatorapp" target="_blank">
              <img src="../../../assets/footing/telegram.png" />
            </a>
          </v-col>
          <v-col cols="1" class="copyright-image">
            <a href="https://twitter.com/marketindicato2" target="_blank">
              <img src="../../../assets/footing/Medium.png" />
            </a>
          </v-col>
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
