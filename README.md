<!--
 * @Author: 章红平
 * @Date: 2023-06-12 20:21:13
 * @LastEditors: 章红平
 * @LastEditTime: 2023-06-13 15:27:59
 * @FilePath: \TruffleLearn\README.md
 * @Description: 描述
-->
快速入门 Truffle

创建项目工程

1.为 Truffle 项目创建新目录：

mkdir MetaCoin
cd MetaCoin(解决网络问题https://www.jianshu.com/p/8c082c8139cd)
2.下载 (“unbox”) MetaCoin box:

truffle unbox metacoin

在操作完成之后，就有这样的一个项目结构：

contracts/: Solidity合约目录

migrations/: 部署脚本文件目录

test/: 测试脚本目录，参考 如何测试应用？

truffle.js: Truffle 配置文件

使用测试
打开控制台终端，运行 Solidity 测试用例:

truffle test ./test/TestMetacoin.sol

运行 JavaScript 测试用例

truffle test ./test/metacoin.js

编译合约
编译智能合约：

truffle compile

使用 Truffle Develop 部署合约

为了部署我们的合约，我们需要连接到区块链网络。Truffle 提供了一个内置的个人模拟区块链，它可以帮助我们用来测试。注意，这个区块链是内地在我们本地的系统里面，他不和以太坊的组网进行连接。

我们可以使用Truffle Develop来创建区块链，并与之交互。

运行 Truffle Develop:

truffle develop


这也显示了10个账号，和他们对你的私钥，这些账号可以用来和区块链进行交互。

在 truffle(develop)> 提示符（因为提供了一个交互式控制台）下， Truffle 的命令可以不带前缀 truffle 执行。比如，可以直接输入compile 来执行truffle compile，以及直接输入 migrate 来部署编译的智能合约到区块链（相当于truffle migrate）：

migrate

显示交易的ID号，部署的合约地址。以及交易的花费和一些相关的状态。

可选: 通过 Ganache 部署

下载安装 Ganache.

编辑器打开 truffle-config.js ，使用下面的内容：

development: {
    host: "127.0.0.1",     // Localhost (default: none)
    port: 7545,            // Standard Ethereum port (default: none)
    network_id: "*",       // Any network (default: none)
},
保存关闭配置文件。

启动Ganache

打开控制台，执行部署：

truffle migrate


在 Ganache 里，点击 “Transactions” 可以看到交易详情。

为了和合约进行交互，我们可以使用 Truffle 的控制台：truffle console， Truffle console 和 Truffle Develop 类似，仅仅是他们连接的链不一样而已，这里是连接 Ganache 。

truffle console
可以看到下面的提示:

truffle(development)>

合约交互
可以使用控制台 console 和合约进行交互：

在 Truffle v5, 控制台支持 async/await 方法（同步方式）, 这样让跟合约交互更简单了，方法如下：

从获取部署合约实例 及获取账号列表 开始：

truffle(development)> let instance = await MetaCoin.deployed()
truffle(development)> let accounts = await web3.eth.getAccounts()
检查账号余额：

truffle(development)> let balance = await instance.getBalance(accounts[0])
truffle(development)> balance.toNumber()
查看以太价值（其实就是调用了一个合约方法：合约方法里定义了一个metacoin 价值 2 ether)：

truffle(development)> let ether = await instance.getBalanceInEth(accounts[0])
truffle(development)> ether.toNumber()
发送一些 metacoin 到其他的账号 :

truffle(development)> instance.sendCoin(accounts[1], 500)
检查刚刚收款人的余额：

truffle(development)> let received = await instance.getBalance(accounts[1])
truffle(development)> received.toNumber()
检查刚刚发送方的余额：

truffle(development)> let newBalance = await instance.getBalance(accounts[0])
truffle(development)> newBalance.toNumber()