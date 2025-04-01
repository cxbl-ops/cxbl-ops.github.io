# 测试环境
- ubuntu22.04
- Geth-1.11.6

<!-- more -->

# 1.拉取代码

```bash
cd /usr/local
git clone https://github.com/ethereum/go-ethereum.git
```
## 切换分支
```bash
git checkout  v1.11.6
```

## 查看当前分支

```bash
git branch
```

## 编译geth

```bash
make geth
```

## 设置软链接

```bash
ln -s /usr/local/go-ethereum/build/bin/geth geth
ln -s /usr/local/go-ethereum/build/bin/bootnode bootnode
```

## 查看版本

```bash
geth version
```

# 2.搭建私链

## 创建数据目录

```bash
cd /mnt
mkdir myeth
cd mygeth
mkdir node1
```

## 创建账号

```bash
geth --datadir node1/ account new
```

## 记录账号密码

```bash
echo "0x00000000000000000000000000000000" >> account.txt
echo '1234' > node1/password.txt
```

## 创建创始区块文件

```bash
vim genesis.json
```

## 内容

```json
{
  "config": {
    "chainId": 7874,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "berlinBlock": 0,
    "londonBlock": 0
  },
  "alloc": {},
  "nonce": "0x0000000000000042",
  "difficulty": "0x20000",
  "coinbase": "0x000000000000000000000000000000000",
  "extraData": "",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "gasLimit": "0x2fefd8",
  "timestamp": "0x00"
}

```

## 查看当前目录

```bash
tree ./
```

## 初始化私链

```bash
geth --datadir node1 init genesis.json
```

## 编写启动脚本(vim genesis.json)

```bash
geth --datadir /mnt/myeth/node1 --identity "node1" --networkid 7874 --http --http.addr "127.0.0.1" --http.port 8545 --http.api "eth,net,web3,personal,miner" --mine --miner.threads=1 --miner.etherbase=0x0000000000000000000000000000000000000000 --allow-insecure-unlock --unlock "0x00000000000000000000000000000000000" --password node1/password.txt > node1.log 2>&1 &
```

## 编写控制台脚本(vim console1.sh)

```bash
geth attach ipc:/mnt/myeth/node1/geth.ipc
```

## 修改权限

```bash
chmod u+rwx ./node1.sh ./console1.sh
```

## 启动脚本

```bash
./node1.sh && ./console1.sh
```
