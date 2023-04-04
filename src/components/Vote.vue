<template>
  <main role="main" class="container">
    <div style="padding-top: 7rem" class="d-none d-lg-block"></div>
    <div class="row justify-content-md-center">
      <div class="col-lg-6">
        <div class="text-center vstack gap-3">
          <h1>2024美国总统大选投票</h1>
          选择一个总统候选人
          <div class="btn-group-vertical" role="group">
            <input
              type="radio"
              class="btn-check"
              name="vbtn-radio"
              id="vbtn-radio1"
              @click="option = 1" />
            <label class="btn btn-outline-dark" for="vbtn-radio1">约翰·博尔顿（John Robert Bolton）</label>
            <input
              type="radio"
              class="btn-check"
              name="vbtn-radio"
              id="vbtn-radio2"
              @click="option = 2"/>
            <label class="btn btn-outline-dark" for="vbtn-radio2">小约瑟夫·罗宾内特·拜登（Joseph Robinette Biden Jr.）</label>
            <input
              type="radio"
              class="btn-check"
              name="vbtn-radio"
              id="vbtn-radio3"
              @click="option = 3"/>
            <label class="btn btn-outline-dark" for="vbtn-radio3">小劳伦斯·约瑟夫·霍根（Lawrence Joseph Hogan Jr.）</label>
            <input
              type="radio"
              class="btn-check"
              name="vbtn-radio"
              id="vbtn-radio4"
              @click="option = 4"/>
            <label class="btn btn-outline-dark" for="vbtn-radio4">蒂姆·斯科特（Timothy Eugene）</label>
          </div>
          <button class="btn btn-info" @click="sendToBlockchain">
            提交投票
          </button>
          <a href="#/" class="btn btn-primary">回退</a>
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { Component, Vue } from "vue-facing-decorator";
import * as ethers from "ethers";
import { calculateMerkleRootAndZKProof } from "zk-merkle-tree";

// the default verifier is for 20 levels, for different number of levels, you need a new verifier circuit
const TREE_LEVELS = 20;

@Component
export default class Vote extends Vue {
  public option = 0;

  async sendToBlockchain() {
    if (!this.option) {
      alert("请选择一个总统候选人!");
      return;
    }

    const commitment = JSON.parse(
      localStorage.getItem("zktree-vote-commitment")
    );
    if (!commitment) {
      alert("未生成承诺, 请进行投票登记!");
      return;
    }

    const abi = [
      "function vote(uint _option,uint256 _nullifier,uint256 _root,uint[2] memory _proof_a,uint[2][2] memory _proof_b,uint[2] memory _proof_c)",
    ];
    const provider = new ethers.providers.Web3Provider(
      (window as any).ethereum
    );
    await provider.send("eth_requestAccounts", []);
    const signer = provider.getSigner();
    const contracts = await (await fetch("contracts.json")).json();
    const contract = new ethers.Contract(contracts.zktreevote, abi, signer);
    const cd = await calculateMerkleRootAndZKProof(
      contracts.zktreevote,
      signer,
      TREE_LEVELS,
      commitment,
      "verifier.zkey"
    );
    try {
      await contract.vote(
        this.option,
        cd.nullifierHash,
        cd.root,
        cd.proof_a,
        cd.proof_b,
        cd.proof_c
      );
    } catch (e) {
      alert(e.reason);
    }
  }
}
</script>