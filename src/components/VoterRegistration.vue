<template>
  <main role="main" class="container">
    <div style="padding-top: 7rem" class="d-none d-lg-block"></div>
    <div class="row justify-content-md-center">
      <div class="col-lg-4">
        <div class="text-center vstack gap-3">
          <a href="#/" class="btn btn-primary">回退</a>
          <h1>您的选票 </h1>
          <img :src="qrcodeDataUrl" />
          <div
            v-if="commitment"
            v-html="splitTwoLines(commitment.commitment)"
          ></div>
          <button class="btn btn-info" @click="copyToClipboard">
            复制到粘贴板
          </button>
          <button class="btn btn-danger" @click="resetCommitment">
            重置选票
          </button>
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { Component, Vue } from "vue-facing-decorator";
import { generateCommitment } from "zk-merkle-tree";
import QRCode from "qrcode";
import copyToClipboard from "copy-to-clipboard";

@Component
export default class VoterRegistration extends Vue {
  public commitment: any = "";
  public qrcodeDataUrl: string = "";

  mounted() {
    this.init();
  }

  async init() {
    this.commitment = JSON.parse(
      localStorage.getItem("zktree-vote-commitment")
    );
    if (!this.commitment) {
      this.commitment = await generateCommitment();
      localStorage.setItem(
        "zktree-vote-commitment",
        JSON.stringify(this.commitment)
      );
    }
    this.qrcodeDataUrl = await QRCode.toDataURL(this.commitment.commitment);
  }

  splitTwoLines(text: string) {
    const half = text.length / 2;
    return (
      text.substring(0, half) + "<br/>" + text.substring(half + 1, text.length)
    );
  }

  public copyToClipboard() {
    copyToClipboard(this.commitment.commitment);
    alert("成功复制到粘贴板");
  }

  public resetCommitment() {
    localStorage.removeItem("zktree-vote-commitment");
    this.init();
  }
}
</script>