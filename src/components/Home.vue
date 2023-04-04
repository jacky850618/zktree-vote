<template>
  <main role="main" class="container">
    <div style="padding-top: 7rem" class="d-none d-lg-block"></div>
    <div class="row justify-content-md-center">
      <div class="col-lg-6">
        <div class="text-center vstack gap-4">
          <h1>2024美国总统大选投票</h1>
          <h3>基于区块链零知识证明的简易投票系统</h3>
          <a href="#/validator" class="btn btn-primary">验证器工具</a>
          <a href="#/registration" class="btn btn-primary">投票登记</a>
          <a href="#/vote" class="btn btn-primary">投票</a>
          <a href="#/results" class="btn btn-primary">投票结果</a>
          <a
            href="https://zhuanlan.zhihu.com/p/619202797">查看文章</a>
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