<script lang="ts" setup>
import { ethers } from "ethers";

const isConnected = ref("");

const connect = async () => {
  try {
    const provider = new ethers.providers.Web3Provider(
      (window as any).ethereum
    );

    let accounts = await provider.listAccounts();
    if (!accounts.length) {
      accounts = await provider.send("eth_accountRequest", []);
    }

    if (!accounts.length) return;

    isConnected.value = accounts[0];
  } catch (error) {
    console.log(error);
  }
};

const disconnect = async () => {
  try {
    isConnected.value = "";
  } catch (error) {
    console.log(error);
  }
};
</script>

<template>
  <ClientOnly>
    <button
      v-if="!isConnected"
      @click="connect()"
      class="bg-black rounded-lg flex items-center justify-center w-fit px-6 py-4 text-white space-x-4 hover:scale-95 hover:bg-black/90 cursor-pointer active:scale-90 active:bg-black/80 transition-all"
    >
      <img
        src="https://cdn.iconscout.com/icon/free/png-256/metamask-2728406-2261817.png"
        class="w-6"
      />
      <span class="text-lg"> MetaMask </span>
    </button>
    <button
      v-else
      @click="disconnect()"
      class="bg-green-400 rounded-lg flex items-center justify-center w-fit px-6 py-4 text-white space-x-4 hover:scale-95 hover:bg-green-400/90 cursor-pointer active:scale-90 active:bg-green-400/80 transition-all"
    >
      Connected with
      {{
        isConnected.substring(0, 4) +
        "..." +
        isConnected.substring(isConnected.length - 4)
      }}
    </button>
  </ClientOnly>
</template>
