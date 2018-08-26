<template>
  <div id="app">
    <Menu />
    <div class="container">
      <Header class="grid"/>
      <Wallet :contacts="contacts" />
    </div>
  </div>
</template>

<script>
  import Header from "./components/Header.vue"
  import Menu from "./components/Menu.vue"
  import Wallet from "./components/Wallet.vue"

  export default {
    name: 'app',
    data () {
      return {
        msg: 'Welcome to Your Vue.js App..',
        contacts:
          [{
              "address":"9egHejbV2z1p1Luy2mER4BXsaHbyM67LdaLrUoJ9YSFRGCw1XPC",
              "alias":"Jane"
          }, {
              "address":"9hHN98zwGKWrttBZ2eCU13g3hpLFXuLvSWFNSJVJiGMtaYRaYiC"
          }, {
              "address":"9gUVQUT5TvYQA3v8cLgHC15T7uN6beB88mbTJ2SdkbDqbSEX5jc"
          }, {
              "address":"9fbzfeeMaMkjKZcRLHkukFP8VffbQBsxNpivjjH4b5oZyASCLuC",
              "alias":"My long lost friend Timothy Bangers The Third"
          }, {
              "address":"9hqxmYPYq2DQ31gasR1smJTnUvwYCyokM9FA7t858JnD8WJFiKJ",
              "alias":"Joshua"
          }, {
              "address": "9gUVQUT5TvYQA3v8cLgHC15T7uN6beB88mbTJ2SdkbDqbSEX5jc",
              "alias": "Robert"
          }, {
              "address": "9eeNsa8cs7ne1kQSbPbM3gfYRxVxzCBJV3mxdTMKriS3byY14Yd"
          }, {
              "address": "9i15dpSxi89V1LMK1WeUiwEnGo9eDJHaxdtoAhr5uDACHGBQzeT"
          }, {
              "address": "9fq9fUiSFNXt5iSMhmzXTzVqgEvFqKe5URAqPxjNArFdxR2nz8p",
              "alias": "Marc"
          }, {
              "address": "9fBxPw7Po3vH3z3oSRwrgxFxKdcFEURYQGQhnTK3Z9npXshjSxr",
              "alias": "Rubio"
          }, {
              "address": "9hzrhdxTviGUrcbNSgFFGp5XNAWd2HupZMrXYJZx5T9r7BxiaH2"
          }, {
              "address": "9gAvgmGFNtisdcDQVzia2kfN7r7c55EkouJorDZNcQYb3iVirdV",
              "alias": "Dan"
          }, {
              "address": "9iQSiCbNTGpMYoRfoTtNLMaYqxNwsw7BXUSzFxXDLSRxNoL2GuE",
              "alias": "Carmack"
          }, {
              "address": "9gsqA9CmLCktedY9eGveVMVzLJnmj9Em2auGiBnYzD7jWssyTtC"
          }, {
              "address": "9griU558xVTxwxSoFvXNXojxgFwBKZGwLoEkDFYx7J7sS1MNXmj",
              "alias": "chatbot"
          }]
      }
    },
    components: {
      Header,
      Menu,
      Wallet,
    },
  }
</script>

<style lang="scss">
  @import './assets/scss/_fonts.scss';
  @import './assets/scss/_globals.scss';
</style>
<style lang="scss" scoped>
  @import './assets/scss/app.scss';
</style>
