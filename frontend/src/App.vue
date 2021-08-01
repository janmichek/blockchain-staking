<template>
  <div
    id="app"
    v-if="!isLoading">
    <header>
      <h1>Staking DAPP</h1>
      Account {{ account }}
      <br>
      Balance: {{ web3.utils.fromWei(balance, 'ether') }}
    </header>
    <main-content
      :account="account"
      :dai-token-balance="daiTokenBalance"
      :dapp-token-balance="daiTokenBalance"
      :staking-balance="stakingBalance"
      :dai-token="daiToken"
      :token-farm="tokenFarm"/>
  </div>
  <div v-else>
    loading
  </div>
</template>

<script>
  import MainContent from './components/MainContent.vue'
  import Web3 from "web3"
  import DaiToken from './contracts/DaiToken.json'
  import DappToken from './contracts/DappToken.json'
  import TokenFarm from './contracts/TokenFarm.json'

  export default {
    name: 'App',
    data () {
      return {
        account: '',
        daiToken: null,
        dappToken: null,
        tokenFarm: null,
        balance: 0,
        daiTokenBalance: 0,
        dappTokenBalance: 0,
        stakingBalance: 0,
        web3: undefined,
        isLoading: true,
      }
    },
    async mounted () {
      await this.loadBlockchainData()
    },
    methods: {
      async loadBlockchainData () {

        if (typeof window.ethereum !== 'undefined') {
          this.web3 = new Web3(window.ethereum)
          const netId = await this.web3.eth.net.getId()
          const accounts = await this.web3.eth.getAccounts()
          this.account = accounts[0]
          //load balance
          if (typeof accounts[0] !== 'undefined') {
            this.balance = await this.web3.eth.getBalance(accounts[0])
          } else {
            window.alert('Please login with MetaMask')
          }

          //load contracts
          try {
            this.daiToken = new this.web3.eth.Contract(DaiToken.abi, DaiToken.networks[netId].address)
            this.dappToken = new this.web3.eth.Contract(DappToken.abi, DappToken.networks[netId].address)
            this.tokenFarm = new this.web3.eth.Contract(TokenFarm.abi, TokenFarm.networks[netId].address)


            this.daiTokenBalance = await this.daiToken.methods.balanceOf(this.account).call()
            this.dappTokenBalance = await this.dappToken.methods.balanceOf(this.account).call()
            this.stakingBalance = await this.tokenFarm.methods.stakingBalance(this.account).call()
          } catch (e) {
            /* eslint-disable no-console*/
            console.log('Error', e)
            window.alert('Contracts not deployed to the current network')
          }
        } else {
          window.alert('Please install MetaMask')
        }

        this.isLoading = false
      },
    },
    components: {
      MainContent,
    },
  }
</script>

