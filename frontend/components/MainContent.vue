<template>
  <div class="container">
    <section>
      <div>
        <strong>Staking Balance</strong>: {{ stakingFormatted }} mDai
      </div>
      <div>
        <strong>Reward Balance</strong>: {{ dappFormatted }} DAPP
      </div>

    </section>
    <div class="panel">
      <div class="panel-heading">
        Stake Tokens
      </div>
      <div class="panel-block">
        Balance: {{ daiFormatted }}
      </div>
      <div class="panel-block">

        <b-field label="Amount">
          <b-input :value="input"></b-input>
        </b-field>


      </div>

<!--      <b-button @click="stake" >STAKE</b-button>-->
    </div>

  </div>
</template>

<script>
  import Web3 from "web3"

  export default {
    name: 'MainContent',
    props: {
      daiTokenBalance: String,
      dappTokenBalance: String,
      stakingBalance: String,
      daiToken: Object,
      tokenFarm: Object,
      w: new Web3(window.ethereum)
    },
    data () {
      return {
        input: '',
      }
    },
    methods: {
      // stake () {
      //   window.web3 = new Web3(window.ethereum)
      //
      //   const amount = window.web3.utils.toWei(this.input.toString())
      //   console.log('amount', amount)
      //   this.daiToken.methods.approve(this.tokenFarm._address, amount)
      //     .send({ from: this.state.account })
      //     .on('transactionHash', () => {
      //       this.tokenFarm.methods.stakeTokens(amount)
      //         .send({ from: this.account })
      //         .on('transactionHash', () => {
      //         })
      //     })
      // },
    },
    computed: {


      stakingFormatted () {
        window.web3 = new Web3(window.ethereum)
        return window.web3.utils.fromWei(this.stakingBalance, 'Ether')
      },
      dappFormatted () {
        window.web3 = new Web3(window.ethereum)
        return window.web3.utils.fromWei(this.dappTokenBalance, 'Ether')
      },
      daiFormatted () {
        window.web3 = new Web3(window.ethereum)
        return window.web3.utils.fromWei(this.daiTokenBalance, 'Ether')
      },
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h3 {
    margin: 40px 0 0;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
