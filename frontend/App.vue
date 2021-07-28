<template>
  <div id="app">
    <the-navbar :account="account"/>
    <main-content
      :dai-token-balance="daiTokenBalance"
      :dapp-token-balance="daiTokenBalance"
      :staking-balance="stakingBalance"
      :dai-token="daiToken"
      :token-farm="tokenFarm"/>
    <b-loading is-full-page v-model="isLoading"/>
  </div>
</template>

<script>
  import MainContent from './components/MainContent.vue'
  import Web3 from "web3"
  import TheNavbar from "./components/TheNavbar"
  import DaiToken from './abis/DaiToken.json'
  import DappToken from './abis/DappToken.json'
  import TokenFarm from './abis/TokenFarm.json'

  export default {
    name: 'App',
    data () {
      return {
        account: '0x0',
        daiToken: {},
        dappToken: {},
        tokenFarm: {},
        daiTokenBalance: '0',
        dappTokenBalance: '0',
        stakingBalance: '0',
        isLoading: true,
      }
    },
    async mounted () {
      await this.loadWeb3()
      await this.loadBlockchainData()
    },
    methods: {
      async loadBlockchainData () {
        const web3 = window.web3
        const accounts = await web3.eth.getAccounts()
        this.account = accounts[0]
        const networkId = await web3.eth.net.getId()

        const daiTokenData = DaiToken.networks[networkId]

        if (daiTokenData) {
          const daiToken = new web3.eth.Contract(DaiToken.abi, daiTokenData.address)
          this.daiToken = daiToken
          let daiTokenBalance = await daiToken.methods.balanceOf(this.account).call()
          this.daiTokenBalance = daiTokenBalance.toString()
        } else {
          window.alert('DaiToken contract not deployed to detected network')
        }

        const dappTokenData = DappToken.networks[networkId]
        if (dappTokenData) {
          const dappToken = new web3.eth.Contract(DappToken.abi, dappTokenData.address)
          this.dappToken = dappToken
          let dappTokenBalance = await dappToken.methods.balanceOf(this.account).call()
          this.dappTokenBalance = dappTokenBalance.toString()
        } else {
          window.alert('dappToken contract not deployed to detected network')
        }


        const tokenFarmData = TokenFarm.networks[networkId]
        if (tokenFarmData) {
          const tokenFarm = new web3.eth.Contract(TokenFarm.abi, tokenFarmData.address)
          this.tokenFarm = tokenFarm
          let stakingBalance = await tokenFarm.methods.stakingBalance(this.account).call()
          this.stakingBalance = stakingBalance.toString()
        } else {
          window.alert('tokenFarm contract not deployed to detected network')
        }

        this.isLoading = false

      },
      async loadWeb3 () {
        if (window.ethereum) {
          window.web3 = new Web3(window.ethereum)
          await window.ethereum.enable()
        } else if (window.web3) {
          window.web3 = new Web3(window.web3.currentProvider)
        } else {
          window.alert('Non-Ethereum browser detected. You should consider trying MetaMask!')
        }
      },
    },
    components: {
      TheNavbar,
      MainContent,
    },
  }
</script>

