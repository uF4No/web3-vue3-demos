<template>
  <div class="container max-">
    <h1>Basic Read</h1>
    <p>
      In this demo, we'll just read data stored in the blockchain. We'll do it
      by calling a method from a smart contract. The smart contract is stored in
      the address
      <span class="font-medium text-blue-600">{{ contractAddress }}</span>
    </p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  setup() {
    const contractAddress = process.env.ADDRESS_BASIC_READ || ''
    return {
      contractAddress,
    }
  },
})
</script>
