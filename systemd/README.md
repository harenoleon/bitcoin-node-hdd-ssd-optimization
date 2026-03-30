# ⚙️ Systemd Services

This directory contains systemd service files for managing the Bitcoin node and related components in a production environment.

---

## 📌 Overview

Using systemd allows:

* Automatic startup on boot
* Process monitoring and auto-restart
* Centralized service management

---

## 🔍 Locate Bitcoin Binary Path

Before using this service, verify the path of Bitcoin Core binaries on your system:

```bash
which bitcoind
which bitcoin-cli
```

If installed manually:
```
find /home/btc -name bitcoind
```
Example:
```
/home/btc/bitcoin-28.0/bin/bitcoind
/home/btc/bitcoin-28.0/bin/bitcoin-cli
```

---

## ⚙️ Service Configuration

```ini
ExecStart=/home/btc/bitcoin-28.0/bin/bitcoind \
  -datadir=/data/bitcoin \
  -conf=/data/bitcoin/bitcoin.conf

ExecStop=/home/btc/bitcoin-28.0/bin/bitcoin-cli \
  -datadir=/data/bitcoin stop
```

---

## 📂 Files

* `bitcoind.service` → Bitcoin Full Node service
* `ckpool.service` *(optional)* → CKPool mining service

---

## 🚀 Installation

Copy the service file to systemd directory:

```bash
sudo cp systemd/bitcoind.service /etc/systemd/system/
```

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Enable service (auto start on boot):

```bash
sudo systemctl enable bitcoind
```

Start service:

```bash
sudo systemctl start bitcoind
```

---

## 📊 Service Management

Check status:

```bash
systemctl status bitcoind
```

View logs:

```bash
journalctl -u bitcoind -f
```

Stop service:

```bash
sudo systemctl stop bitcoind
```

Restart service:

```bash
sudo systemctl restart bitcoind
```

---

## ⚠️ Notes

* Custom services should be placed in `/etc/systemd/system/`
* Ensure the `Youruser` user exists on the system
* Verify the path to `bitcoind` binary (`which bitcoind`)
* Make sure `/data/bitcoin` directory exists and is accessible

---

## 🧠 Why systemd?

Using systemd ensures:

* Reliable service management
* Automatic recovery if the node crashes
* Cleaner deployment compared to manual execution

---
