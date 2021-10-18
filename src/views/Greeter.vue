<template>
  <div class="container max-">
    <h1 class="text-xl mb-8 font-bold">Basic Read & write</h1>
    <p>
      First, we'll just read a greet message stored in the blockchain. We'll do
      it by calling the greet method from a smart contract. The smart contract
      is stored in the address
      <span class="font-medium text-blue-600">{{ contractAddress }}</span>
    </p>
    <p><strong>Reading data from the blockchain is free 🤑</strong></p>
    <button
      @click="getGreeting()"
      type="button"
      class="inline-flex items-center px-4 py-2 my-8 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
    >
      <svg
        class="-ml-1 mr-2 h-5 w-5"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 20 20"
        fill="currentColor"
        aria-hidden="true"
      >
        <path
          d="M2.003 5.884L10 9.882l7.997-3.998A2 2 0 0016 4H4a2 2 0 00-1.997 1.884z"
        />
        <path d="M18 8.118l-8 4-8-4V14a2 2 0 002 2h12a2 2 0 002-2V8.118z" />
      </svg>
      Get message
    </button>

    <p class="mb-4">
      The message is
      <span class="text-blue-500 font-medium">{{ message }}</span>
    </p>
    <hr />
    <h2 class="text-xl my-8 font-bold">Write to the blockchain</h2>
    <p>
      Enter a new message to be written in the blockchain. This message will
      replace the existing one.
    </p>
    <p>
      <strong
        >Writting data to the blockchain requires a fee. Metamask will request
        it.</strong
      >
    </p>
    <div class="max-w-lg mx-auto mt-4 text-left">
      <label for="message" class="block text-sm font-medium text-gray-700"
        >New message</label
      >
      <div class="mt-1">
        <input
          v-model="newMessage"
          type="text"
          name="message"
          id="message"
          class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
          placeholder="Enter a new message here"
        />
      </div>
    </div>

    <button
      @click="saveGreeting()"
      type="button"
      class="inline-flex items-center px-4 py-2 my-8 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-pink-600 hover:bg-pink-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-pink-500"
    >
      <svg
        class="-ml-1 mr-2 h-5 w-5"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 20 20"
        fill="currentColor"
        aria-hidden="true"
      >
        <path
          d="M2.003 5.884L10 9.882l7.997-3.998A2 2 0 0016 4H4a2 2 0 00-1.997 1.884z"
        />
        <path d="M18 8.118l-8 4-8-4V14a2 2 0 002 2h12a2 2 0 002-2V8.118z" />
      </svg>
      Save message
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import { ethers } from 'ethers'

// Smart contract facade
import Greeter from '@/artifacts/solidity/contracts/Greeter.sol/Greeter.json'

export default defineComponent({
  setup() {
    const contractAddress = process.env.VUE_APP_ADDRESS_GREETER || ''
    const message = ref('')
    const newMessage = ref('')

    const getGreeting = async () => {
      //@ts-expect-error Window.ethers not TS
      if (typeof window.ethereum !== 'undefined') {
        //@ts-expect-error Window.ethers not TS
        const provider = new ethers.providers.Web3Provider(window.ethereum)
        const contract = new ethers.Contract(
          contractAddress,
          Greeter.abi,
          provider
        )
        try {
          const data = await contract.greet()
          console.log('greet :>> ', data)
          message.value = data
        } catch (error) {
          console.error(error)
        }
      }
    }
    const saveGreeting = async function() {
      //@ts-expect-error Window.ethers not TS
      if (typeof window.ethereum !== 'undefined') {
        //@ts-expect-error Window.ethers not TS
        const provider = new ethers.providers.Web3Provider(window.ethereum)
        // get the account that will pay for the trasaction
        const signer = provider.getSigner()
        // as the operation we're going to do is a transaction,
        // we pass the signer instead of the provider
        const contract = new ethers.Contract(
          contractAddress,
          Greeter.abi,
          signer
        )
        try {
          const transaction = await contract.setGreeting(newMessage.value)

          console.log('transaction :>> ', transaction)
          // wait for the transaction to actually settle in the blockchain
          await transaction.wait()
          newMessage.value = ''
        } catch (error) {
          console.error(error)
        }
      }
    }
    return {
      contractAddress,
      message,
      newMessage,
      getGreeting,
      saveGreeting,
    }
  },
})
</script>
