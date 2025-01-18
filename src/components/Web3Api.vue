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
</template>

<script setup>
import { ref } from 'vue'
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

async function main() {
  try {
    // 獲取帳戶列表
    const accounts = await web3.eth.getAccounts()
    console.log('帳戶列表:', accounts)

    // 假設有帳戶，查詢第一個帳戶的餘額
    if (accounts.length > 0) {
      balance.value = await web3.eth.getBalance(address)
      console.log(`帳戶 ${address.value} 的餘額:`, balance.value)
    } else {
      console.log('沒有可用的帳戶')
    }
  } catch (error) {
    // 錯誤處理
    console.error('操作失敗:', error.message)
  }
}

main()
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
