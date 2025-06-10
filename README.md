Here’s your complete **README.md** file—ready to drop into a new GitHub repo and guide users through setting up a Neova storage provider node via Docker on Ubuntu:

````markdown
# 🔗 Neova Provider Node (Ubuntu + Docker)

Use this guide to set up and run a **Neova Storage Provider Node** using Docker on Ubuntu. It includes installation, configuration, monitoring, and optional systemd service setup.

---

## 🐳 Prerequisites

- Ubuntu (>=20.04 LTS), 64-bit  
- CPU: ≥ 2 cores  
- RAM: ≥ 4 GB  
- SSD: ≥ 50 GB (to contribute storage)  
- **EVM wallet address** (checksummed, `0x...`, 42 chars)  
- **IPFS Cluster Peer ID** (e.g., `12D3...`)  
- Docker privileges  

---

## ✅ Step 1: Install Docker

```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
  | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
   https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
sudo systemctl enable --now docker
````

---

## 📥 Step 2: Pull Neova Provider Image

```bash
sudo docker pull neovaprotocol/provider
```

---

## 🛑 Step 3: Remove Existing Container (If Any)

```bash
sudo docker rm -f neova || true
```

---

## 🔑 Step 4: Run the Provider Container

Replace placeholders with your real values:

```bash
sudo docker run -d \
  --name neova \
  --restart always \
  -e EVM_ADDRESS=0xYourChecksumEVMAddress \
  -e IPFS_PEER_ID=Your12D3PeerID \
  neovaprotocol/provider
```

Ensure:

* `EVM_ADDRESS` is checksummed (uppercase/lowercase mix).
* `IPFS_PEER_ID` matches your actual IPFS peer ID.

---

## 📋 Step 5: Monitor Registration Logs

```bash
sudo docker logs -f neova
```

Look for:

```
✅ Signature generated successfully
🚀 Provider registered successfully
```

---

## 📊 Step 6: Verify Node Health & Earning

After registration, the node will:

* Shard and encrypt your storage allocation
* Contribute uptime and storage capacity
* Earn **\$NEOV tokens**

You can monitor:

* Storage utilization
* Uptime
* Earnings via CLI or dashboard

---

## ⚙️ Step 7: Optional – systemd Service

Create a service file for auto-start on reboot:

```ini
# /etc/systemd/system/neova-provider.service
[Unit]
Description=Neova Provider Node
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker start -a neova
ExecStop=/usr/bin/docker stop -t 2 neova

[Install]
WantedBy=multi-user.target
```

Enable it:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now neova-provider
```

---

## 🛠 Troubleshooting

* **HTTP 000 registration errors**: Check network, SSL, DNS, and firewall settings—`peer-api.neova.io:443` must be reachable.
* **400 Bad Request**: Validate that both your `EVM_ADDRESS` and `IPFS_PEER_ID` are correctly formatted and exactly match the real values.
* **Finding IPFS peer ID**:

  If you run IPFS Cluster locally, execute:

  ```bash
  ipfs-cluster-service id
  ```

  Or inspect the file:

  ```bash
  cat <cluster_dir>/identity.json | grep '"id"'
  ```

---

## 🧭 CLI Reference

| Purpose                   | Command                           |
| ------------------------- | --------------------------------- |
| View logs                 | `sudo docker logs -f neova`       |
| Exec into container shell | `sudo docker exec -it neova sh`   |
| Restart the provider      | `sudo docker restart neova`       |
| View systemd logs         | `journalctl -u neova-provider -f` |

---

## 🎉 License & Contributing

Licensed under the **MIT License**. Feel free to contribute through issues and pull requests.

---

**Start contributing storage to the Neova network—and begin earning \$NEOV today!**

```

---

📌 **Next Steps**:  
1. Create your GitHub repository.  
2. Add this `README.md` file.  
3. Adjust placeholders like EVM address and peer ID.  
4. Commit and push!

Let me know if you’d like a `.gitignore`, license file, or example CI setup added to the repo.
```
