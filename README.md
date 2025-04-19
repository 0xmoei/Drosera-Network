# Drosera-Network

In this Guide, we contribute to the Drosera testnet by:
- Installing the CLI
- Setting up a vulnerable contract
- Deploying a Trap on testnet
- Connecting an operator to the Trap

## Recommended System Requirements
- 2 CPU Cores
- 4 GB RAM
- 20 GB Disk Space

## Official Discord
[Join here](https://discord.gg/UXAdpTYjgr)

## Install Dependencies

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

```bash
sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

### Docker

```bash
sudo apt update -y && sudo apt upgrade -y

for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update

sudo apt-get install ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world
```

---

# Trap Setup

## 1. Configure Environments 

### Drosera CLI

```bash
curl -L https://app.drosera.io/install | bash
```

```bash
source /root/.bashrc
```

```bash
droseraup
```

### Foundry CLI

```bash
curl -L https://foundry.paradigm.xyz | bash
```

```bash
source /root/.bashrc
```

```bash
foundryup
```

### Bun

```bash
curl -fsSL https://bun.sh/install | bash
```

---

## 2. Deploy Contract & Trap

```bash
mkdir my-drosera-trap
```

```bash
cd my-drosera-trap
```

### Replace Github_Email & Github_Username:

```bash
git config --global user.email "Github_Email"
git config --global user.name "Github_Username"
```

### Initialize Trap:

```bash
forge init -t drosera-network/trap-foundry-template
```

### Compile Trap:

```bash
curl -fsSL https://bun.sh/install | bash
bun install
```

```bash
forge build
```

> Skip warnings!

### Deploy Trap:

```bash
DROSERA_PRIVATE_KEY=xxx drosera apply
```

- Replace `xxx` with your EVM wallet `privatekey` (ensure it’s funded with Holesky ETH).
- When prompted, write `ofc` and press Enter.

  ![image](https://github.com/user-attachments/assets/6d1161f1-4423-4ce6-a1a2-77ce567186dc)

### Note:

If you encounter rate-limiting errors (e.g., HTTP 429) or JSON-RPC errors (e.g., -32000) with the default RPC in `drosera.toml`, update the `ethereum_rpc` field to use an Infura RPC URL. Sign up at [Infura](https://infura.io/), create a project, and get your Holesky endpoint (e.g., `https://holesky.infura.io/v3/YOUR_INFURA_KEY`).

Edit `drosera.toml`:

```bash
cd my-drosera-trap
nano drosera.toml
```

Update the `ethereum_rpc` line:

```toml
ethereum_rpc = "https://holesky.infura.io/v3/YOUR_INFURA_KEY"
```

Save and re-run:

```bash
DROSERA_PRIVATE_KEY=xxx drosera apply
```

---

## 3. Check Trap in Dashboard

Connect your Drosera EVM wallet: [https://app.drosera.io/](https://app.drosera.io/)

Click on `Traps Owned` to see your deployed Traps OR search your Trap address.

![image](https://github.com/user-attachments/assets/9c39eea0-0aaf-417d-8552-765ff33f8a5e)

---

## 4. Bloom Boost Trap

Open your Trap on the Dashboard and click on `Send Bloom Boost` to deposit some Holesky ETH.

![image](https://github.com/user-attachments/assets/2f5216fd-fdf9-4732-96d0-959b3fbce479)

---

## 5. Fetch Blocks

```bash
drosera dryrun
```

---

# Operator Setup

## 1. Whitelist Your Operator

### Edit Trap Configuration:

```bash
cd my-drosera-trap
nano drosera.toml
```

Add the following codes at the bottom of `drosera.toml`:

```toml
private_trap = true
whitelist = ["Operator_Address"]
```

- Replace `Operator_Address` with your EVM wallet `Public Address` between `" "` symbols.
- Your `Public Address` is your `Operator_Address`.

### Update Trap Configuration:

```bash
DROSERA_PRIVATE_KEY=xxx drosera apply
```

- Replace `xxx` with your EVM wallet `privatekey`.

Your Trap should be private now with your operator address whitelisted internally.

![image](https://github.com/user-attachments/assets/9ae6d58e-3be7-4d0d-9c4b-3b486224df4e)

---

## 2. Operator CLI

```bash
cd ~
```

### Download:

```bash
curl -LO https://github.com/drosera-network/releases/releases/download/v1.16.2/drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz
```

### Install:

```bash
tar -xvf drosera-operator-v1.16.2-x86_64-unknown-linux-gnu.tar.gz
```

### Test the CLI:

```bash
./drosera-operator --version
```

```bash
sudo cp drosera-operator /usr/bin
```

```bash
drosera-operator
```

---

## 3. Install Docker Image

```bash
docker pull ghcr.io/drosera-network/drosera-operator:latest
```

---

## 4. Register Operator

```bash
drosera-operator register --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com --eth-private-key PV_KEY
```

- Replace `PV_KEY` with your Drosera EVM privatekey. Use the same wallet as your trap wallet.

---

## 5. Open Ports

```bash
sudo ufw allow ssh
sudo ufw allow 22
sudo ufw enable

sudo ufw allow 31313/tcp
sudo ufw allow 31314/tcp
```

---

## 6. Install & Run Operator

Choose one Installation Method:

- Method 1: Install using Docker
- Method 2: Install using SystemD

---

### Method 1: Docker

#### Configure Docker:

```bash
git clone https://github.com/0xmoei/Drosera-Network
cd Drosera-Network
cp .env.example .env
nano .env
```

- Replace `your_evm_private_key` with your EVM wallet privatekey.
- Replace `your_vps_public_ip` with your VPS public IP address.

#### Run Operator:

```bash
docker compose up -d
```

#### Check Health:

```bash
cd Drosera-Network
docker compose logs -f
```

![image](https://github.com/user-attachments/assets/2ec4d181-ac60-4702-b4f4-9722ef275b50)

>  No problem if you are receiving `WARN drosera_services::network::service: Failed to gossip message: InsufficientPeers`
---

### Method 2: SystemD

#### Configure SystemD Service File:

Enter this command in the terminal, but first replace:
- `PV_KEY` with your privatekey.
- `VPS_IP` with your VPS IP (without anything else).
- If using a local system, replace VPS IP with `0.0.0.0`.

```bash
sudo tee /etc/systemd/system/drosera.service > /dev/null <<EOF
[Unit]
Description=drosera node service
After=network-online.target

[Service]
User=$USER
Restart=always
RestartSec=15
LimitNOFILE=65535
ExecStart=$(which drosera-operator) node --db-file-path $HOME/.drosera.db --network-p2p-port 31313 --server-port 31314 \
    --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com \
    --eth-backup-rpc-url https://1rpc.io/holesky \
    --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 \
    --eth-private-key PV_KEY \
    --listen-address 0.0.0.0 \
    --network-external-p2p-address VPS_IP \
    --disable-dnr-confirmation true

[Install]
WantedBy=multi-user.target
EOF
```

#### Run Operator:

```bash
sudo systemctl daemon-reload
sudo systemctl enable drosera
sudo systemctl start drosera
```

#### Check Node Health:

```bash
journalctl -u drosera.service -f
```

![image](https://github.com/user-attachments/assets/a4ad6e66-4749-4780-9347-c878399d4067)

> !! No problem if you are receiving `WARN drosera_services::network::service: Failed to gossip message: InsufficientPeers`
---

## 7. Opt-in Trap

In the dashboard, click on `Opt-in` to connect your operator to the Trap.

![image](https://github.com/user-attachments/assets/5189b5cb-cb46-4d10-938a-33f71951dfc2)

---

## 8. Check Node Liveness

Your node will start producing green blocks in the dashboard.

![image](https://github.com/user-attachments/assets/9ad08265-0ea4-49f7-85e5-316677245254)
