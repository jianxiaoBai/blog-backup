---
title: 以太坊 笔记
date: 2018-07-11 23:33:38
tags: [以太坊, 智能合约, 去中心化, 区块链]
keywords: [以太坊, 智能合约, 去中心化, 区块链]
categories: [技术笔记]
comments: false
---

## 常用操作

```
eth.accounts                                                      => 查看账户
personal.newAccount()                                             => 创建账户
eth.getBalance(eth.accounts[0])                                   => 查看余额
miner.start(1)                                                    => 启动挖矿
miner.stop()                                                      => 停止挖矿
miner.start(1);admin.sleepBlocks(1);miner.stop();                 => 挖矿后
web3.fromWei(eth.getBalance(eth.accounts[1]))                     => 查看转换过后的数量
geth init ./genesis.json --datadir ./data                         => 初始化
geth --datadir ./data --nodiscover --networkid 1024 --rpc console => 启动
geth --datadir ./data --nodiscover --networkid 1024 --rpc --rpccorsdomain="\*" console => 启动时解决本地跨域问题
```

<!-- more -->

## 坑点

- `60000 * (5000000000 / 1e18) = 60000 * (gasLimit / 1e18)` 计算打币需消耗的 eth
- 地址前面需要加上 0x 否则生成地址不一致的问题
- chainId 不一致带来的交易失效问题
- `--rpccorsdomain="*"` 解决跨域的问题
- 批量打 true (动态设置 nonce 值)
- 通过 `web3.eth.sendTransaction` 发起交易可以记录 tx 交易值

## ERC20 API

```
eth      => 包含一些跟操作区块链相关的方法
net      => 包含以下查看p2p网络状态的方法
admin    => 包含一些与管理节点相关的方法
miner    => 包含启动&停止挖矿的一些方法
personal => 主要包含一些管理账户的方法
txpool   => 包含一些查看交易内存池的方法
web3     => 包含了以上对象，还包含一些单位换算的方法
```

## 钱包概念

以太坊系钱包名词有地址、密码、私钥、助记词、keystore。

若以银行账户为类比，这 5 个词分别对应内容如下：

```
地址          = 银行卡号
密码          = 银行卡密码
私钥          = 银行卡号+银行卡密码
助记词        = 银行卡号+银行卡密码
Keystore+密码 = 银行卡号+银行卡密码
Keystore      ≠ 银行卡号
```
