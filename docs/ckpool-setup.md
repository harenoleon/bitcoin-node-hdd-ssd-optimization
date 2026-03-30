# ⛏️ CKPool Setup Guide (Production Ready)

This guide covers installation and deployment of CKPool integrated with a Bitcoin Full Node.

---

## 📌 Overview

CKPool is a lightweight mining pool server that connects directly to your Bitcoin node.

**Use case:**

* Solo mining
* Pool infrastructure learning
* Full node + mining integration

---

## ⚙️ Prerequisites

Make sure your Bitcoin node is running:

```bash id="m9v7fj"
bitcoin-cli -datadir=/data/bitcoin getblockchaininfo
```

---

## 📥 1. Install Dependencies

```bash id="3y3d1y"
sudo apt update
sudo apt install -y git build-essential autoconf automake libtool pkg-config \
libcurl4-openssl-dev libjansson-dev libssl-dev
```

---

## 📦 2. Clone CKPool

Source: https://bitbucket.org/ckolivas/ckpool/src/master/

```bash id="j0db4b"
cd /home/btc
git clone https://bitbucket.org/ckolivas/ckpool.git
cd ckpool
```

---

## 🔨 3. Build CKPool

```bash id="q2n5l3"
./autogen.sh
./configure
make
```

---

## ⚙️ 4. Configure CKPool

Create config file:

```bash id="p2r3f1"
nano ckpool.conf
```

Example:

```json id="u9v1xx"
{
  "poolname": "MyBitcoinPool",
  "serverurl": "127.0.0.1:8332",
  "rpcuser": "bitcoinrpc",
  "rpcpassword": "yourpassword",
  "threads": 4,
  "logdir": "logs"
}
```

📌 Ensure your `bitcoin.conf` has:

```ini id="8t9m1a"
server=1
rpcuser=bitcoinrpc
rpcpassword=yourpassword
```

---

## ▶️ 5. Run CKPool

```bash id="d6k3x2"
./ckpool -c ckpool.conf
```

---

## 📊 6. Monitor Logs

```bash id="n3l8f9"
tail -f logs/ckpool.log
```

---

## 🔍 7. Verify Connection

Check if CKPool connects to Bitcoin node:

```bash id="e8w2k1"
bitcoin-cli getmininginfo
```

---

## 🔄 8. Run as Service (Optional)

Create service file:

```bash id="h1k9z4"
sudo nano /etc/systemd/system/ckpool.service
```

```ini id="x5c2m7"
[Unit]
Description=CKPool Mining Pool
After=network.target

[Service]
User=btc
WorkingDirectory=/home/btc/ckpool
ExecStart=/home/btc/ckpool/ckpool -c /home/btc/ckpool/ckpool.conf
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable service:

```bash id="r4t7p2"
sudo systemctl daemon-reload
sudo systemctl enable ckpool
sudo systemctl start ckpool
```

---

## 📈 Troubleshooting

### Check logs

```bash id="v8k2l1"
tail -f logs/ckpool.log
```

### Check Bitcoin RPC

```bash id="y2m5n8"
bitcoin-cli getblockchaininfo
```

### Common Issues

* RPC authentication mismatch
* Bitcoin node not fully synced
* Port 8332 not accessible

---

## 🧠 Summary

* CKPool connects to local Bitcoin node via RPC
* Used for mining or infrastructure experimentation
* Can be deployed as a background service

---
