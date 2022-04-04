<template>
  <div class="landing">
    <div class="topvar d-flex align-center flex-column">
      <p class="topvar-text-1">Find the right place to invest your crypto</p>
      <img id="object1" src="../assets/landing/hero.png" alt="" />
      <img id="object2" src="../assets/landing/crypto.png" alt="" />
      <p class="topvar-text-2">
        MIDEFI is an algorithmic distributed protocol which allows users to borrow and lend crypto
        currencies
      </p>
      <button class="button">Invest today</button>
    </div>
    <v-row class="secvar">
      <p class="sec-text-1">As easy as 1,2,3</p>
      <p class="sec-text-2">Connect your wallet and start earning money in a few steps</p>
      <img id="object3" src="../assets/landing/frame1.png" />
      <img id="object4" src="../assets/landing/frame2.png" />
      <v-row class="d-flex align-center justify-center sec-image">
        <v-col
          cols="3"
          class="d-flex align-center justify-center flex-column"
          style="padding-bottom: 30px"
        >
          <img src="../assets/landing/all.png" />
          <p class="sec-text-3">Connect Wallet</p>
        </v-col>
        <v-col cols="3" class="d-flex align-center justify-center flex-column">
          <img src="../assets/landing/find.png" />
          <p class="sec-text-4">
            Use our simple interface to supply or borrow from markets in a few clicks
          </p>
        </v-col>
        <v-col cols="3" class="d-flex align-center justify-center flex-column">
          <img src="../assets/landing/grow.png" />
          <p class="sec-text-5">
            Supply tokens to earn interest with no risk, you can withdraw them whenever you want
          </p>
        </v-col>
      </v-row>
    </v-row>
    <div class="container">
      <v-row class="d-flex align-center justify-center">
        <v-col cols="5" class="simp">
          <v-row class="text-2">
            <p>Join the DeFi revolution on the MIDeFi Network</p>
          </v-row>
          <v-row class="my-3 simple-text-2">
            Enjoy the possibilities that Open Finance offers in a safe, clear and understandable way
          </v-row>
        </v-col>
        <v-col cols="4" class="hidden-sm-and-down justify-center align-center">
          <img
            class="landing-1"
            src="https://www.marketindicator.app/assets_files/roadmap-illustration2.png"
            alt=""
          />
        </v-col>
      </v-row>

      <v-row class="d-flex align-center justify-center">
        <v-col cols="6" class="simp">
          <img
            src="https://www.marketindicator.app/assets_files/roadmap-illustration1.png"
            alt=""
            width="400px"
            height="422.86px"
          />
        </v-col>
        <v-col cols="4" class="hidden-sm-and-down justify-center align-center">
          <v-row class="text-2">
            <p>Experience the basics of lending and borrowing assets</p>
          </v-row>
          <v-row class="my-3 simple-text-2">
            Explore what you can do with your crypto assets and be part of the change to a more
            financially inclusive world
          </v-row>
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Landing',
  methods: {
    toMyActivity() {
      this.$router.push({ name: 'MyActivity' })
    },
    toGithub() {
      window.location.href = 'https://github.com/riflending/'
      this.$router.push('https://github.com/riflending/')
    },
    scrollToTop() {
      window.scrollTo(0, 0)
      const button = document.getElementById('connectButton')
      if (button.click) {
        button.click()
      }
    },
    sayhello() {
      alert('Hello')
    },
  },
}
</script>

<style>
#object1 {
  position: absolute;
  left: 67.5%;
  right: 8.88%;
  top: 8.27%;
  bottom: 84.69%;

  mix-blend-mode: luminosity;
}
#object2 {
  position: absolute;
  left: 0%;
  right: 65.42%;
  top: 7.78%;
  bottom: 70.09%;
}
#object3 {
  position: absolute;
  width: 107px;
  height: 87px;
  left: 97px;
  top: 1186px;
}
#object4 {
  position: absolute;
  width: 120px;
  height: 98px;
  left: 1300px;
  top: 800px;
}
</style>
