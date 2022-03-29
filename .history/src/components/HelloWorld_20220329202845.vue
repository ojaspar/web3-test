<template>
  <div>
    <div class="card">
      <div>
        <label for="contact">Contact Address</label>
        <input type="text" id="contact" v-model="contractAddress" />
      </div>

      <div>
        <label for="amount">Amount</label>
        <input type="text" id="amount" v-model="amount" />
      </div>

      <div>
        <label for="destination">Destination Address</label>
        <input type="text" id="destination" v-model="destinationAddress" />
      </div>

      <button>Transfer</button>
    </div>
  </div>
</template>

<script>
import data from '../abi.data';
import Web3 from 'web3';
import { onMounted, ref } from 'vue';
export default {
  setup() {
    const ABI = ref(data);
    const contractAddress = ref('0x78536177b32FCcaF12b98EEb33e8e815D4DD1712');
    const destinationAddress = ref('');
    const amount = ref('');

    const initialLoadWeb3 = async () => {
      if (window.ethereum) {
        window.web3 = new Web3(window.ethereum);
        window.ethereum.enable();
      }
    };

    const loadWeb3 = async () => {
      await initialLoadWeb3();
      window.contract = await loadContract();
      await transfer();
    };

    const loadContract = async () => {
      return await new window.web3.eth.Contract(
        ABI.value,
        contractAddress.value
      );
    };

    const transfer = async () => {
      const tx = await new window.contract.methods.transfer(
        '0x86C12A724340f3F4F6142789808874d0A55B',
        0
      ).send({ from: window.ethereum.selectedAddress });
      console.log(tx);
    };
    onMounted(() => {
      loadWeb3();
    });
    return { destinationAddress, amount, contractAddress, ABI };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card {
  background: #ffffff;
  box-shadow: 6px 6px 54px #0000000d;
  border-radius: 14px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 500px;
  padding: 20px;
}
input {
  height: 40px;
  width: 100px;
  outline: none;
  border: 1px solid blue;
  border-radius: 10px;
}
label {
  width: 100%;
}
</style>
