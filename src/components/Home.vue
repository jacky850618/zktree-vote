<template>
  <main role="main" class="container">
    <div style="padding-top: 7rem" class="d-none d-lg-block"></div>
    <div class="row justify-content-md-center">
      <div class="col-lg-6">
        <div class="text-center vstack gap-4">
          <h1>2024美国总统大选投票</h1>
          <h3>基于区块链零知识证明的简易投票系统</h3>
          <a href="#/validator" class="btn btn-primary large">身份验证(验证人员专用)</a>
          <a href="#/registration" class="btn btn-primary large">我的选票</a>
          <a href="#/vote" class="btn btn-primary large">我要投票</a>
          <a href="#/results" class="btn btn-primary large">投票结果</a>
          <a href="https://zhuanlan.zhihu.com/p/619202797">查看文章</a>
        </div>
      </div>
      <div class="col-lg-6">
        <div class="vstack">
          <pre>
          限制条件:
          1. 因为每个人都只能投票一次;
          2. 没有人知道哪张选票分配给了哪个选民;

          操作流程:
          1. 角色: 投票人、验证人
          1. 过程:
            1>. 投票人的验证: 投票人可以亲自到投票地点进行，也可以通过视频会议系统等方式在线完成。
            验证人可以使用<a href="#/validator">验证人工具</a>进行验证。当验证者通过检查选民
            的身份证等方式检查选民的身份时，他向区块链发送一个带有唯一哈希的选票，该哈希可以是用户
            身份证的哈希或任何唯一标识符。这确保了一名选民只能通过一项选票登记一次，这意味着她只能
            投票一次。
            2>. 选票(commitment)的发送: 可以通过扫描选民的二维码将选票发送给验证者，或者选民可
            以复制它并在聊天中发送（在线验证）。
            3>. 选票(commitment)的生成: 打开页面时系统会在后台自动生成选票和无效符，并将其存储
            在浏览器的本地存储中。
            4>. 投票: 投票过程很简单。 您选择一个选项并将其发送到区块链。 该应用程序将使用在后台生成的无效符。
          </pre>
        </div>
      </div>
    </div>
  </main>
</template>

<script lang="ts">
import { Component, Vue } from "vue-facing-decorator";

@Component
export default class Home extends Vue { }
</script>