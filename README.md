# zktree-vote
使用零知识证明构建基于以太坊区块链的简易匿名投票系统

## Usage

- 通过 `https://github.com/jacky850618/zktree-vote` 克隆代码仓
- 在项目目录下通过`npm install` 安装项目依赖和准备
- 在项目目录中通过 `npx hardhat node` 启动一个 Hardhat 节点
- 在另一个终端通过 `npm run deploy` 部署智能合约
- 通过 `npm start` 启动应用程序

该应用程序使用 MetaMask 连接区块链，因此必须安装 MetaMask 扩展，并连接到 Hardhat 本地节点。 
智能合约所有者是第一个 Hardhat 账户，第二个账户被部署脚本设置为验证者。

有关更多详细信息，请阅读我的[知乎文章](https://zhuanlan.zhihu.com/p/619202797)
