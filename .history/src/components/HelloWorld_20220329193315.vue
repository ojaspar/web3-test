<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br />
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener"
        >vue-cli documentation</a
      >.
    </p>
    <h3>Installed CLI Plugins</h3>
    <ul>
      <li>
        <a
          href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel"
          target="_blank"
          rel="noopener"
          >babel</a
        >
      </li>
      <li>
        <a
          href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint"
          target="_blank"
          rel="noopener"
          >eslint</a
        >
      </li>
    </ul>
    <h3>Essential Links</h3>
    <ul>
      <li>
        <a href="https://vuejs.org" target="_blank" rel="noopener">Core Docs</a>
      </li>
      <li>
        <a href="https://forum.vuejs.org" target="_blank" rel="noopener"
          >Forum</a
        >
      </li>
      <li>
        <a href="https://chat.vuejs.org" target="_blank" rel="noopener"
          >Community Chat</a
        >
      </li>
      <li>
        <a href="https://twitter.com/vuejs" target="_blank" rel="noopener"
          >Twitter</a
        >
      </li>
      <li>
        <a href="https://news.vuejs.org" target="_blank" rel="noopener">News</a>
      </li>
    </ul>
    <h3>Ecosystem</h3>
    <ul>
      <li>
        <a href="https://router.vuejs.org" target="_blank" rel="noopener"
          >vue-router</a
        >
      </li>
      <li>
        <a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a>
      </li>
      <li>
        <a
          href="https://github.com/vuejs/vue-devtools#vue-devtools"
          target="_blank"
          rel="noopener"
          >vue-devtools</a
        >
      </li>
      <li>
        <a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener"
          >vue-loader</a
        >
      </li>
      <li>
        <a
          href="https://github.com/vuejs/awesome-vue"
          target="_blank"
          rel="noopener"
          >awesome-vue</a
        >
      </li>
    </ul>
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
      updateStatus('Ready');
      await transfer();
    };

    const updateStatus = async (status) => {
      console.log(status);
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
// import Web3 from 'web3';
// export default {
//  name: 'HelloWorld',
//   props: {
//     msg: String,
//   },
//   data: () => ({
//     ABI: data,
//     contractAddress: '0x78536177b32FCcaF12b98EEb33e8e815D4DD1712',
//     destinationAddress: '',
//     amount: '',
//   }),
//   methods: {
//     async initialLoadWeb3() {
//       if (window?.ethereum) {
//         // try {
//         //   const account = await window.ethereum.request({
//         //     method: 'eth_requestAccounts',
//         //   });
//         //   this.loadContract(account[0]);
//         //   console.log(account);
//         // } catch (e) {
//         //   console.log(e);
//         // }
//         console.log(window.ethereum);
//         window.web3 = new Web3(window.ethereum);
//         window.ethereum.enable();
//       }
//     },
//     async load() {
//       await this.initialLoadWeb3();
//       window.contract = await this.loadContract();
//       await this.performTransfer();
//     },
//     async loadContract() {
//       // console.log(new window.web3.eth.Contract());
//       return await new window.web3.eth.Contract(this.ABI, this.contractAddress);
//     },

//     async performTransfer() {
//       console.log(window.ethereum.selectedAddress);
//       const ptx = await window.contract.methods
//         .transfer('0x86C12A724340f3F4F6142789808874d0A55B', 0)
//         .send({ from: window.ethereum.selectedAddress });
//       console.log(ptx);
//     },
//   },
//   mounted() {
//     this.load();
//   },
// }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
