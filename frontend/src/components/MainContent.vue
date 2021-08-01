<template>
  <div>
    <section>
      <div>
        <strong>Staking Balance</strong>: {{ stakingFormatted }} mDAI
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
    </section>
    <hr>
    <section>
      <h2>
        Unstake Tokens
      </h2>
      <button @click="unstake">UNSTAKE ALL</button>
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
        web3: new Web3(window.ethereum),
      }
    },
    methods: {
      stake () {
        const amount = this.web3.utils.toWei(this.input)
        /* eslint-disable no-console*/

        console.log('amount', amount)
        this.daiToken.methods.approve(this.tokenFarm._address, amount)
          .send({ from: this.account })
          .on('transactionHash', () => {
            this.tokenFarm.methods.stakeTokens(amount)
              .send({ from: this.account })
              .on('transactionHash', () => {
                window.location.reload(true)
              })
          })
      },

      unstake () {
        this.tokenFarm.methods.unstakeTokens()
          .send({ from: this.account })
          .on('transactionHash', () => {
          window.location.reload(true)
        })
      },
    },
    computed: {
      stakingFormatted () {
        return this.web3.utils.fromWei(this.stakingBalance, 'Ether')
      },
      dappFormatted () {
        return this.web3.utils.fromWei(this.dappTokenBalance, 'Ether')
      },
      daiFormatted () {
        return this.web3.utils.fromWei(this.daiTokenBalance, 'Ether')
      },
    },
  }
</script>
