<template>
  <h1>{{ title }}</h1>

  <p>
    address : <span class="value">{{ address }}</span>
  </p>
  <p>
    privateKey : <span class="value">{{ privateKey }}</span>
  </p>
  <p>
    帳戶餘額 : <span class="value">{{ balance }}</span>
  </p>
  <p>
    帳戶次數 : <span class="value">{{ nonce }}</span>
  </p>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Web3 from 'web3'

const title = ref('Account Info')

//實例化 web3
const web3 = new Web3(Web3.givenProvider || 'https://mainnet.infura.io/v3/7a9a90e210894f7ca4c7fc17b5fb567b')
// console.log(web3)

//建立帳號，每執行一次就會建立新的帳號，所以執行一次拿到 address、privateKey 要註解
// const { address, privateKey } = web3.eth.accounts.create('test123')

const address = ref('0xF80C1f19847026CA3df18b82c5376134Ab254c28')
const privateKey = ref('0xdfa1aa88104217a4ee2a96c60f6045413b9d578c3a7b95235e0e13af1e16d621')
const balance = ref(0)

//單位轉換

const balanceInWei = '1000000000000000000' // 區塊鏈返回的餘額（wei）
const balanceInEther = web3.utils.fromWei(balanceInWei, 'ether') // 轉換成 ether
console.log(`你的餘額是: ${balanceInEther} ETH`) // 輸出: 你的餘額是: 1 ETH

const amountInEther = '0.5' // 用戶想發送 0.5 ether
const amountInWei = web3.utils.toWei(amountInEther, 'ether') // 轉換成 wei
console.log(`發送金額: ${amountInWei} wei`) // 輸出: 發送金額: 500000000000000000 wei

//手續費
const gasPriceInGwei = '20'
const gasPriceInWei = web3.utils.toWei(gasPriceInGwei, 'Gwei') // 轉換成 wei
const gasLimit = 21000 // 標準交易的 gasLimit
const feeInWei = gasPriceInWei * gasLimit
console.log(`手續費: ${feeInWei} wei`)

// 獲取餘額
async function fetchBalance() {
  try {
    const balanceInWei = await web3.eth.getBalance(address.value)
    balance.value = web3.utils.fromWei(balanceInWei, 'ether')
  } catch (error) {
    console.error('獲取餘額失敗:', error)
  }
}

// 獲取交易數量
const nonce = ref(0)
async function fetchTransactionCount() {
  try {
    const res = await web3.eth.getTransactionCount(address.value)
    console.log('Transaction Count:', res)

    nonce.value = res
  } catch (error) {
    console.error('獲取交易數量失敗:', error)
  }
}

// 在組件掛載時執行
onMounted(() => {
  fetchBalance()
  fetchTransactionCount()
})
</script>

<style lang="scss" scoped>
p {
  font-size: 1.5rem;
  margin-bottom: 1rem;

  .value {
    color: #fff;
    font-size: 1rem;
    font-weight: 500;
  }
}
</style>
