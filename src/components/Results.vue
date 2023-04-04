<template>
  <main role="main" class="container">
    <div style="padding-top: 7rem" class="d-none d-lg-block"></div>
    <div class="row justify-content-md-center">
      <div class="col-lg-4">
        <div class="text-center vstack gap-3">
          <h1>2024美国总统大选投票结果</h1>
          <div class="text-start">约翰·博尔顿（John Robert Bolton）{{ options[0] }}</div>
          <div class="text-start">小约瑟夫·罗宾内特·拜登（Joseph Robinette Biden Jr.）: {{ options[1] }}</div>
          <div class="text-start">小劳伦斯·约瑟夫·霍根（Lawrence Joseph Hogan Jr.）: {{ options[2] }}</div>
          <div class="text-start">蒂姆·斯科特（Timothy Eugene）: {{ options[3] }}</div>
          <a href="#/" class="btn btn-primary">回退</a>
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { Component, Vue } from "vue-facing-decorator";
import * as ethers from "ethers";

@Component
export default class Vote extends Vue {
  public options = [0, 0, 0, 0];

  mounted() {
    this.init();
  }

  async init() {
    const abi = [
      "function getOptionCounter(uint _option) external view returns (uint)",
    ];
    const provider = new ethers.providers.Web3Provider(
      (window as any).ethereum
    );
    const signer = provider.getSigner();
    const contracts = await (await fetch("contracts.json")).json();
    const contract = new ethers.Contract(contracts.zktreevote, abi, signer);
    for (let i = 0; i < 4; i++) {
      this.options[i] = (await contract.getOptionCounter(i + 1)).toString();
    }
    console.log(this.options[3]);
  }
}
</script>