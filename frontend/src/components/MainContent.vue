<template>
  <div>
    <section>
      <div>
        <strong>Staking Balance</strong>: {{ stakingFormatted }} mDai
      </div>
      <div>
        <strong>Reward Balance</strong>: {{ dappFormatted }} DAPP
      </div>
    </section>

    <section>
      <h2>
        Stake Tokens
      </h2>
      <div>
        Balance: {{ daiFormatted }}
      </div>
      <div>
        <label>Amount</label>
        <input v-model="input"/>
      </div>

      <button @click="stake">STAKE</button>
      <button @click="unstake">UNSTAKE</button>
    </section>
  </div>
</template>

<script>
  import Web3 from "web3"

  export default {
    name: 'MainContent',
    props: {
      account: String,
      daiTokenBalance: String,
      dappTokenBalance: String,
      stakingBalance: String,
      daiToken: Object,
      tokenFarm: Object,
    },
    data () {
      return {
        input: '',
      }
    },
    methods: {
      stake () {
        // window.web3 = new Web3(window.ethereum)
        const web3 = window.web3

        const amount = web3.utils.toWei(this.input)
        console.log('amount', amount)
        this.daiToken.methods.approve(this.tokenFarm._address, amount)
          .send({ from: this.account })
          .on('transactionHash', () => {
            this.tokenFarm.methods.stakeTokens(amount)
              .send({ from: this.account })
              .on('transactionHash', () => {
              })
          })
      },

      unstake (){
        this.tokenFarm.methods.unstakeTokens().send({ from: this.account }).on('transactionHash', () => {
        })
      }
    },
    computed: {
      stakingFormatted () {
        const web3 = window.web3
        return web3.utils.fromWei(this.stakingBalance, 'Ether')
      },
      dappFormatted () {
        const web3 = window.web3
        return web3.utils.fromWei(this.dappTokenBalance, 'Ether')
      },
      daiFormatted () {
        const web3 = window.web3
        return web3.utils.fromWei(this.daiTokenBalance, 'Ether')
      },
    },
  }
</script>
