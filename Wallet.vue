<template>
  <div style="background:#11131a;min-height:100vh;display:flex;flex-direction:column;align-items:center;justify-content:center;color:#0fffc7">
    <h2>RA Atum – Vue Wallet Connect</h2>
    <button class="connect-btn" @click="connectWallet">Connect Wallet</button>
    <div v-if="address" style="margin-top:18px;">Connected: {{ shortAddress }}</div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Use your WalletConnect Project ID
const projectId = "f3ef3bd3e385e3eaf6e73a71c18556e1";

const chains = [
  {
    chainId: 1,
    name: 'Ethereum',
    rpcUrl: 'https://mainnet.infura.io/v3/'
  },
  {
    chainId: 56,
    name: 'Binance Smart Chain',
    rpcUrl: 'https://bsc-dataseed.binance.org/'
  }
]

const address = ref("");

// Only create modal if running in browser
let modal;
if (typeof window !== "undefined" && window.Web3Modal) {
  modal = new window.Web3Modal({
    projectId,
    chains: chains.map(chain => ({
      id: chain.chainId,
      name: chain.name,
      rpcUrl: chain.rpcUrl
    })),
    themeMode: 'dark',
    themeVariables: {
      '--w3m-accent': '#0fffc7',
      '--w3m-background': '#11131a',
    }
  });
}

const connectWallet = async () => {
  try {
    const connection = await modal.connect();
    const accounts = await connection.accounts();
    address.value = accounts[0];
  } catch (e) {
    address.value = "";
  }
};

const shortAddress = computed(() =>
  address.value ? `${address.value.slice(0, 6)}...${address.value.slice(-4)}` : ''
)
</script>

<style>
.connect-btn {
  padding: 16px 34px;
  background: linear-gradient(90deg, #0fffc7, #00c3ff);
  color: #11131a;
  border: none;
  border-radius: 50px;
  font-size: 1.2rem;
  cursor: pointer;
  margin-bottom: 30px;
}
.connect-btn:hover {
  background: linear-gradient(90deg, #00c3ff, #0fffc7);
}
</style>
