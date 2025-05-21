---
title: "sui localnet setup"
date: 2025-05-21T19:40:38+08:00
draft: false
tags: ["Sui", "区块链", "本地环境", "Docker"]
categories: ["开发环境", "区块链"]
---

在本教程中，我们将通过 Docker 快速启动一个本地的 Sui 节点，并配置好客户端环境用于测试开发。

## 环境准备

确保你已安装好 Docker，推荐版本 20.10 或更高。

将适用 ubuntu 的 sui 的发布包下载到本地，并解压到当前目录的 `sui` 文件夹下。

## 启动本地节点

使用以下命令拉起一个本地节点环境：

- 在执行这个命令之前，你应该。

```bash
docker run --rm -it \
 -v `pwd`/sui:/data/bin \
 -p 9000:9000 \
 -p 9123:9123 \
 -e RUST_LOG="off,sui_node=info" \
 image.styleofwong.com/ubuntu \
 /data/bin/sui start --with-faucet --force-regenesis
```

该命令将：

- 将当前目录下的 `sui` 文件夹挂载到容器中的 `/data/bin`

- 映射本地端口 9000 和 9123

- 设置日志等级

- 启动节点并开启 Faucet 水龙头

## 配置客户端

启动完节点后，打开新的终端执行以下命令：

```bash
sui client new-env --alias local --rpc http://127.0.0.1:9000
sui client switch --env local
```

这将：

- 创建一个名为 `local` 的本地环境配置

- 切换到该环境进行开发

## 获取测试币

访问水龙头链接并替换其中的地址为你自己的钱包地址：

```bash
# 替换为你自己的地址
address="0x9bb949dcd30d982ffd8e7373e1945b2f78440951057557c98608bde073d2d165"
curl 'http://127.0.0.1:9123/v2/gas' \
  -H 'Content-Type: application/json' \
  --data-raw '{"FixedAmountRequest":{"recipient":"'${address}'"}}'
```

## 结语

现在你已经成功在本地启动了一个 Sui 测试环境，可以开始智能合约和链上交互的开发了！
