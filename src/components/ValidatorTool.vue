<template>
  <main role="main" class="container">
    <div style="padding-top: 7rem" class="d-none d-lg-block"></div>
    <div class="row justify-content-md-center">
      <div class="col-lg-4">
        <div class="text-center vstack gap-3">
          <h1>2024美国总统大选投票验证器工具</h1>
          <div id="reader" width="400px"></div>
          <input type="text" placeholder="承诺" v-model="commitment" />
          <input type="text" placeholder="唯一哈希" v-model="uniqueHash" />
          <button class="btn btn-info" @click="sendToBlockchain">
            提交验证
          </button>
          <a href="#/" class="btn btn-primary">回退</a>
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { Component, Vue } from "vue-facing-decorator";
import { Html5QrcodeScanner } from "html5-qrcode";
import * as ethers from "ethers";

@Component
export default class ValidatorTool extends Vue {
  public commitment = "";
  public uniqueHash = "";

  mounted() {
    let html5QrcodeScanner = new Html5QrcodeScanner(
      "reader",
      { fps: 10, qrbox: { width: 400, height: 400 } },
      false
    );
    html5QrcodeScanner.render(
      (decodedText, decodedResult) => {
        this.commitment = decodedText;
      },
      (error) => {
        // console.log(error);
      }
    );
  }

  async sendToBlockchain() {
    const abi = [
      "function registerCommitment(uint256 _uniqueHash, uint256 _commitment)",
    ];
    const provider = new ethers.providers.Web3Provider(
      (window as any).ethereum
    );
    await provider.send("eth_requestAccounts", []);
    const signer = provider.getSigner();
    const contracts = await (await fetch("contracts.json")).json();
    const contract = new ethers.Contract(contracts.zktreevote, abi, signer);
    if (!this.commitment) {
      alert("需要承诺(Commitment)");
      return;
    }
    if (!this.uniqueHash) {
      alert("需要唯一哈希(Unique hash)");
      return;
    }
    try {
      await contract.registerCommitment(this.uniqueHash, this.commitment);
    } catch (e) {
      alert(e.reason);
    }
  }
}
</script>