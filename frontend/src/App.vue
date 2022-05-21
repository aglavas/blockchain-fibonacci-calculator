<template>
  <div id="app">
    <div class="container">
      <h1 class="text-center">Fibonacci</h1>
      <div class="row">
        <div class="col-sm-12">
          <div class="form-group">
            <label for="number">Fibonacci sequence of</label>
            <input v-model="fibNumber" type="number" class="form-control" id="number"/>
          </div>
          <br>
          <button @click="fib" class="btn btn-primary">Submit</button>
          <p class="h1">{{ result }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Fibonacci from '../../build/contracts/Fibonacci.json';
import { getWeb3 } from './utils.js';
export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      web3: null,
      contract: null,
      fibNumber: null,
      result: null
    }
  },
  async mounted() {
    const web3 = await getWeb3();
    const networkId = await web3.eth.net.getId();
    const deployedNetwork = Fibonacci.networks[networkId];
    const contract = new web3.eth.Contract(
      Fibonacci.abi,
      deployedNetwork && deployedNetwork.address,
    );
    this.web3 = web3;
    this.contract = contract;
  },
  methods: {
    async fib() {
      if (!this.fibNumber) {
        return false;
      }
      this.result = await this.contract.methods.fib(this.fibNumber).call();
    }
  }
};
</script>
