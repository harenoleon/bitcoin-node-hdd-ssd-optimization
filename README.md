# bitcoin-node-hdd-ssd-optimization

# 🚀 Bitcoin Full Node (HDD + SSD Optimization)

## 📌 Overview

This project demonstrates how to deploy and optimize a Bitcoin Full Node by separating storage workloads between HDD and SSD to improve synchronization performance.

**Key Idea:**

* HDD → store large sequential data (`blocks`)
* SSD → store high random I/O data (`chainstate`, `indexes`)

This reduces disk bottlenecks and significantly improves sync speed.

---

## 🧠 Key Insight

Bitcoin node sync is **disk I/O bound**, not CPU bound.

Moving `chainstate` and `indexes` to SSD improves:

* Block verification speed
* UTXO database access
* Overall sync performance

---

## 🧱 Architecture

```
HDD (/data/bitcoin)
├── blocks/
└── wallet/

SSD (/home/btc/bitcoin_fast)
├── chainstate/
└── indexes/
```

---

## ⚙️ Storage Mapping

```bash
-v /data/bitcoin:/bitcoin/.bitcoin \
-v /data/bitcoin/blocks:/bitcoin/.bitcoin/blocks \
-v /home/btc/bitcoin_fast/chainstate:/bitcoin/.bitcoin/chainstate \
-v /home/btc/bitcoin_fast/indexes:/bitcoin/.bitcoin/indexes
```

---

## 🔗 Symlink Optimization

```bash
ln -s /home/btc/bitcoin_fast/chainstate /data/bitcoin/chainstate
ln -s /home/btc/bitcoin_fast/indexes /data/bitcoin/indexes
```

> Redirect high I/O operations to SSD for better performance

---

## ▶️ Quick Start

### Start Node

```bash
./scripts/start.sh
```

### Stop Node

```bash
./scripts/stop.sh
```

---

## 📊 Monitoring

```bash
# blockchain status
bitcoin-cli -datadir=/data/bitcoin getblockchaininfo

# live log
tail -f /data/bitcoin/debug.log

# disk activity
iostat -xm 5 | grep sda
```

---

## 🌐 Peer Optimization

```bash
bitcoin-cli getpeerinfo | jq '.[].pingtime'
```

* Custom script to ban slow peers (> 0.4 latency)

---

## ⛏️ Mining Pool Integration

Integrated with CKPool:

* Source: https://bitbucket.org/ckolivas/ckpool/src/master/
* Log monitoring:

```bash
tail -f /home/btc/master/logs/ckpool.log
```

---

## 📊 Dashboard

Bitcoin Node Manager:
https://github.com/Mirobit/bitcoin-node-manager

---

## 🛠️ Project Structure

```
.
├── README.md
├── docs/
│   └── setup.md
├── config/
│   └── bitcoin.conf
├── scripts/
│   ├── start.sh
│   ├── stop.sh
│   └── monitor.sh
└── systemd/
    └── bitcoind.service
```

---

## 📘 Full Setup Guide

👉 See: `docs/setup.md`
Includes:

* Storage preparation
* Symlink configuration
* Bitcoin installation
* Node deployment
* CKPool integration

---

## 📈 Performance Improvement

| Setup     | Result                     |
| --------- | -------------------------- |
| HDD only  | Slow sync                  |
| HDD + SSD | Faster verification & sync |

---

## 🔐 Access

```bash
ssh btc@192.168.xx.xxx
```

---

## 🚧 Future Improvements

* Docker Compose deployment
* Prometheus + Grafana monitoring
* Auto peer management
* Alert system (disk / node health)

---

## 👨‍💻 Author

Built as a personal infrastructure and blockchain optimization project
Focused on system performance, storage architecture, and node operation.
