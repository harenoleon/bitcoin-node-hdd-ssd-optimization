
# ⚙️ Full Setup Guide (Production Ready)

This guide covers full deployment of a Bitcoin Full Node with HDD + SSD optimization.

---

## 📌 System Requirements

* OS: Ubuntu 20.04+ / Debian
* CPU: 4+ cores
* RAM: 8–16 GB

**Storage:**

* HDD (≥ 1TB) → blockchain data
* SSD (≥ 200GB) → chainstate + indexes

---

## ⚡ Quick Setup

### 1. Install Bitcoin Core

```bash
sudo apt update
sudo apt install bitcoind bitcoin-cli -y
```

---

### 2. Prepare Storage

```bash
mkdir -p /data/bitcoin
mkdir -p /home/btc/bitcoin_fast
```

---

### 3. Create Optimized Structure

```bash
mkdir -p /data/bitcoin/blocks
mkdir -p /home/btc/bitcoin_fast/chainstate
mkdir -p /home/btc/bitcoin_fast/indexes
```

---

### 4. Symlink (SSD Optimization)

```bash
ln -s /home/btc/bitcoin_fast/chainstate /data/bitcoin/chainstate
ln -s /home/btc/bitcoin_fast/indexes /data/bitcoin/indexes
```

---

### 5. Start Node

```bash
bitcoind -datadir=/data/bitcoin -daemon
```

---

## 📈 Performance Result

* Before (HDD only): slow sync
* After (HDD + SSD): faster verification

---

## 🧠 Summary

* Install → Prepare → Optimize → Run
* SSD is used for high I/O workloads
* HDD is used for large storage

---
