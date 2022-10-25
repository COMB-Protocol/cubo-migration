<script setup lang="ts">
import { ethers } from "ethers";
import intent from "~~/shared/abis/intent";

const isLoading = ref(false);
const hasDecision = ref(false);
const error = ref(undefined);

const decide = async (decision: number) => {
  try {
    const provider = new ethers.providers.Web3Provider(
      (window as any).ethereum
    );

    const intents = new ethers.Contract(
      "0x0e20dd1c407ce5329883e486eb09a0f6da093fe9",
      intent,
      provider.getSigner()
    );

    const tx = await intents.decide(decision);

    isLoading.value = true;

    await tx.wait();

    await   checkDecision();
  } catch (error) {
    error.value = error.toString();
    console.log(error.value);
  } finally {
    isLoading.value = false;
  }
};

const checkDecision = async () => {
  const provider = new ethers.providers.Web3Provider((window as any).ethereum);

  const intents = new ethers.Contract(
    "0x0e20dd1c407ce5329883e486eb09a0f6da093fe9",
    intent,
    provider.getSigner()
  );

  hasDecision.value =
    (await intents.userStatus(provider.getSigner().getAddress())) == 1;
};

onMounted(() => {
  checkDecision();
  (window as any).ethereum.on("accountsChanged", () => {
    checkDecision();
  });
});
</script>

<template>
  <div class="space-y-6">
    <WelcomeSection />
    <div class="space-y-6 mx-4 sm:mx-0">
      <div>
        <h1 class="text-2xl font-bold">Step 1. Connect your wallet</h1>
      </div>
      <div class="flex">
        <MetaMaskConnectButton />
      </div>
      <div class="space-y-2">
        <h1 class="text-2xl font-bold">Step 2. Choose an option</h1>
        <div class="border p-4 rounded-lg space-y-4">
          <div>
            <span>
              For more information regarding the CUBO migration to COMB, please
              head over to our
              <a
                href="https://discord.gg/combfinancial"
                class="underline text-green-700"
              >
                Discord
              </a>
              and read the announcements. After you have made a decision on this
              webpage, you will be airdropped tokens at a later date.
            </span>
            <br />
          </div>
          <div class="text-green-700">
            Make sure you are connected to the Polygon network.
          </div>
          <div v-if="isLoading">Confirming...</div>
          <div
            v-else-if="!hasDecision"
            class="border p-2 w-fit rounded-md bg-green-400/20"
          >
            You have already made a decision.
          </div>
          <div v-else class="flex space-x-4">
            <button
              @click="decide(2)"
              class="p-2 border text-black rounded-md px-8 py-2 hover:opacity-90 active:scale-95 transition-all"
            >
              Exit
            </button>
            <button
              @click="decide(1)"
              class="p-2 border bg-green-700 text-white rounded-md px-12 py-2 hover:opacity-90 active:scale-95 transition-all"
            >
              Migrate
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
