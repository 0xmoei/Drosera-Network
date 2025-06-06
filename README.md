# Drosera-Network
In this Guide, we contribute to Drosera testnet by:
1. Installing the CLI
2. Setting up a vulnerable contract
3. Deploying a Trap on testnet
4. Connecting an operator to the Trap

# Recommended System Requirements
* 2 CPU Cores
* 4 GB RAM
* 20 GB Disk Space
* Get started with a low-budget `VPS` for as low as $5! [Purchase here](https://my.hostbrr.com/order/forms/a/NTMxNw==)
* Create your own `Ethereum Holesky RPC` in [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/).

### Install Dependecies
```
sudo apt-get update && sudo apt-get upgrade -y
```
```
sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```
### Install Docker
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

<h1 align="center">Trap Setup</h1>

## 1. Configure Enviorments
**Drosera CLI**:
```bash
curl -L https://app.drosera.io/install | bash
```
```
echo 'export PATH="$HOME/drosera/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
```
```
droseraup
```

**Foundry CLI**:
```
curl -L https://foundry.paradigm.xyz | bash
```
```
echo 'export PATH="$HOME/.foundry/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
```
```
foundryup
```

**Bun:**
```
curl -fsSL https://bun.sh/install | bash
```
```
source /root/.bashrc
```

---

## 2. Deploy Contract & Trap
```bash
mkdir my-drosera-trap
```
```bash
cd my-drosera-trap
```
**Replace `Github_Email` & `Github_Username`:**
```bash
git config --global user.email "Github_Email"
git config --global user.name "Github_Username"
```
**Initialize Trap**:
```
forge init -t drosera-network/trap-foundry-template
```
**Compile Trap**:
```bash
curl -fsSL https://bun.sh/install | bash

source /root/.bashrc

bun install
```
```bash
forge build
```
* skip warnings!

![image](https://github.com/user-attachments/assets/ab0bd1ec-c2b5-473a-adee-6c2d4975ba05)


**Deploy Trap**:
```bash
DROSERA_PRIVATE_KEY=xxx drosera apply
```
* Replace `xxx` with your EVM wallet `privatekey` (Ensure it's funded with `Holesky ETH`)
* Enter the command, when prompted, write `ofc` and press Enter.

![image](https://github.com/user-attachments/assets/6d1161f1-4423-4ce6-a1a2-77ce567186dc)

#

* **Existing Users**: If you've deployed a trap with your wallet previously, then you need to add your trap address to `drosera.toml` file, by adding this line: `address = "TRAP_ADDRESS"` in the bottom.
  * Ensure you replace `TRAP_ADDRESS` with your own trap address.
  * If you don't know how to get your trap address, Follow this [step](https://github.com/0xmoei/Drosera-Network/blob/main/README.md#-error-apply-config-failed).
  * Finally, as an existing user, Ensure you filled out `drosera.toml` bottom lines like this:
  ```
  whitelist = ["Operator1_Address","Operator2_Address"]
  address = "TRAP_ADDRESS"
  ```
  * After modifying your `drosera.toml`, re-apply your trap configuration by executing this command: `DROSERA_PRIVATE_KEY=xxx drosera apply`

#

🚨 **Error**: You may get several errors (.eg #429) due to `rpc` issues, to fix, you can enter bellow command by adding `--eth-rpc-url`
```bash
DROSERA_PRIVATE_KEY=xxx drosera apply --eth-rpc-url RPC
```
* Replace `RPC` with your own Ethereum Holesky rpc by registering and creating one in [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/).

---

## 3. Check Trap in Dashboard
1- Connect your Drosera EVM wallet: https://app.drosera.io/

2- Click on `Traps Owned` to see  your deployed Traps OR search your Trap address.

![image](https://github.com/user-attachments/assets/9c39eea0-0aaf-417d-8552-765ff33f8a5e)

---

## 4. Bloom Boost Trap
Open your Trap on Dashboard and Click on `Send Bloom Boost` and deposit some `Holesky ETH` on it.

![image](https://github.com/user-attachments/assets/2f5216fd-fdf9-4732-96d0-959b3fbce479)

## 5. Fetch Blocks
```bash
drosera dryrun
```

---

<h1 align="center">Operator Setup</h1>

## 1. Whitelist Your Operator
**1- Edit Trap configuration:**
```bash
cd my-drosera-trap
nano drosera.toml
```
Add the following codes at the bottom of `drosera.toml`:
```toml
private_trap = true
whitelist = ["Operator_Address"]
```
* Replace `Operator_Address` with your EVM wallet `Public Address` between " " symbols
* Your `Public Address` is your `Operator_Address`.

**2- Update Trap Configuration:**
```bash
DROSERA_PRIVATE_KEY=xxx drosera apply
```
* Replace `xxx` with your EVM wallet `privatekey`
* If RPC issue, use `DROSERA_PRIVATE_KEY=xxx drosera apply --eth-rpc-url RPC` and replace `RPC` with your own.

Your Trap should be private now with your operator address whitelisted internally.

![image](https://github.com/user-attachments/assets/9ae6d58e-3be7-4d0d-9c4b-3b486224df4e)

---

## 2. Operator CLI
```bash
cd ~
```
```bash
# Download
curl -LO https://github.com/drosera-network/releases/releases/download/v1.17.2/drosera-operator-v1.17.2-x86_64-unknown-linux-gnu.tar.gz

# Install
tar -xvf drosera-operator-v1.17.2-x86_64-unknown-linux-gnu.tar.gz
```
* Currently the Operator CLI version is `v1.17.2`. Verify the latest version [here](https://github.com/drosera-network/releases/releases)
* You have to get the link of `drosera-operator-v1.x.x-x86_64-unknown-linux-gnu.tar.gz`

# Move path to run it globally
`mv drosera-operator ~/drosera/bin/`
`echo 'export PATH="$HOME/drosera/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc`

Test the CLI with `./drosera-operator --version` to verify it's working.
```console
# Check version
./drosera-operator --version

# Check if it is working
drosera-operator
```

## 3. Install Docker image
```
docker pull ghcr.io/drosera-network/drosera-operator:latest
```

---

## 4. Register Operator
```bash
drosera-operator register --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com --eth-private-key PV_KEY
```
* Replace `PV_KEY` with your Drosera EVM `privatekey`. We use the same wallet as our trap wallet.

---

## 5. Open Ports
```bash
# Enable firewall
sudo ufw allow ssh
sudo ufw allow 22
sudo ufw enable

# Allow Drosera ports
sudo ufw allow 31313/tcp
sudo ufw allow 31314/tcp
```

---

## 6. Install & Run Operator
**Choose one Installation Method:**
* Method 1: [Install using Docker](https://github.com/0xmoei/Drosera-Network/blob/main/README.md#method-1-docker)
* Method 2: [Install using SystemD](https://github.com/0xmoei/Drosera-Network/blob/main/README.md#method-2-systemd)

## Method 1: Docker
### 6-1-1: Configure Docker
* Make sure you have installed `Docker` in Dependecies step.

If you are currently running via old `systemd` method, stop it:
```
sudo systemctl stop drosera
sudo systemctl disable drosera
```
```
git clone https://github.com/0xmoei/Drosera-Network
```
```
cd Drosera-Network
```
```
cp .env.example .env
```
Edit `.env` file:
```
nano .env
```
* Replace `your_evm_private_key` and `your_vps_public_ip`
* To save: `CTRL`+`X`, `Y` & `ENTER`.

Edit `docker-compose.yaml` file:
```bash
nano docker-compose.yaml
```
* Replace default `--eth-rpc-url` to your private [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/) Ethereum Holesky RPCs.
* To save: `CTRL`+`X`, `Y` & `ENTER`.

### 6-1-2: Run Operator
```
docker compose up -d
```

### 6-1-3: Check health
```
docker logs -f drosera-node
```

![image](https://github.com/user-attachments/assets/2ec4d181-ac60-4702-b4f4-9722ef275b50)

>  No problem if you are receiveing `WARN drosera_services::network::service: Failed to gossip message: InsufficientPeers`

### 6-1-4: Optional Docker commands
```console
# Stop node
cd Drosera-Network
docker compose down -v

# Restart node
cd Drosera-Network
docker compose up -d
```

**Now running your node using `Docker`, you can Jump to step 7.**

---

## Method 2: SystemD
### 6-2-1: Configure SystemD service file
Enter this command in the terminal, But first replace:
* `PV_KEY` with your `privatekey`
* `VPS_IP` with your solid vps IP (without anything else)
* Replace default `https://ethereum-holesky-rpc.publicnode.com` to your private [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/) Ethereum Holesky RPCs.
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

### 6-2-2: Run Operator
```console
# reload systemd
sudo systemctl daemon-reload
sudo systemctl enable drosera

# start systemd
sudo systemctl start drosera
```

### 6-2-3: Check Node Health
```console
journalctl -u drosera.service -f
```

![image](https://github.com/user-attachments/assets/a4ad6e66-4749-4780-9347-c878399d4067)

> !! No problem if you are receiveing `WARN drosera_services::network::service: Failed to gossip message: InsufficientPeers`

### 6-2-4: Optional commands
```console
# Stop node
sudo systemctl stop drosera

# Restart node
sudo systemctl restart drosera
```
**Now running your node using `SystemD`, you can Jump to step 7.**
---

## 7. Opt-in Trap
In the dashboard., Click on `Opti in` to connect your operator to the Trap

![image](https://github.com/user-attachments/assets/5189b5cb-cb46-4d10-938a-33f71951dfc2)

---

## 8. Check Node Liveness
Your node will start producing greeen blocks in the dashboard

![image](https://github.com/user-attachments/assets/9ad08265-0ea4-49f7-85e5-316677245254)

---

# Run 2nd Operators
Let's run a 2nd operator on your Trap using `Docker`:
### 1- Stop SystemD if ruuning operator using it.
```bash
sudo systemctl stop drosera
sudo systemctl disable drosera
```

### 2- Whitelist Your 2nd Operator
1- Create a new EVM wallet, Fund it with `Holeksy ETH` and write down its Privatekey (`2nd_Operator_Privatekey`) & PublicAddress (`2nd_Operator_Address`).

2- Edit Trap Configuration:
```
cd ~/my-drosera-trap
nano drosera.toml
```
Edit the following codes at the bottom of `drosera.toml`:
```toml
private_trap = true
whitelist = ["1st_Operator_Address","2nd_Operator_Address"]
```
* Replace `1st_Operator_Address` & `2nd_Operator_Address` with your Operators EVM wallet Public Addresses between " " symbols.

3- Update Trap Configuration:
```bash
DROSERA_PRIVATE_KEY=xxx drosera apply
```
* Replace `xxx` with your `Trap` EVM wallet `privatekey`.
* If RPC issue, use `DROSERA_PRIVATE_KEY=xxx drosera apply --eth-rpc-url RPC` and replace `RPC` with your own.

Your `2nd_Operator_Address` is now added to the whitelist in the dashboard of your Trap.

![image](https://github.com/user-attachments/assets/76b2e12f-5b8e-463a-9bc1-f176f9719d00)

![image](https://github.com/user-attachments/assets/c9f7fd1f-6c91-4322-b9ca-c4b3cc35aa8e)

### 3- Register 2nd Operator
```bash
drosera-operator register --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com --eth-private-key 2nd_Operator_Privatekey
```
* Replace `2nd_Operator_Privatekey` with your 2nd Operator Privatekey.

### 4- Open Ports 
```console
sudo ufw allow 31315/tcp
sudo ufw allow 31316/tcp
```

### 5- Edit `docker-compose.yaml`
```
cd Drosera-Network
```
```
nano docker-compose.yaml
```
Replace the content of the file with the following codes:
```yaml
version: '3'
services:
  drosera1:
    image: ghcr.io/drosera-network/drosera-operator:latest
    container_name: drosera-node1
    network_mode: host
    volumes:
      - drosera_data1:/data
    command: node --db-file-path /data/drosera.db --network-p2p-port 31313 --server-port 31314 --eth-rpc-url RPC_URL_1 --eth-backup-rpc-url https://holesky.drpc.org --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 --eth-private-key ${ETH_PRIVATE_KEY} --listen-address 0.0.0.0 --network-external-p2p-address ${VPS_IP} --disable-dnr-confirmation true
    restart: always

  drosera2:
    image: ghcr.io/drosera-network/drosera-operator:latest
    container_name: drosera-node2
    network_mode: host
    volumes:
      - drosera_data2:/data
    command: node --db-file-path /data/drosera.db --network-p2p-port 31315 --server-port 31316 --eth-rpc-url RPC_URL_2 --eth-backup-rpc-url https://holesky.drpc.org --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 --eth-private-key ${ETH_PRIVATE_KEY2} --listen-address 0.0.0.0 --network-external-p2p-address ${VPS_IP} --disable-dnr-confirmation true
    restart: always

volumes:
  drosera_data1:
  drosera_data2:
```
* Replace `RPC_URL_1` & `RPC_URL_2` with your own Ethereum Holesky RPC, You can use [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/) or even a reliable public rpc of holesky network to create Ethereum Holesky RPC.
* You can use same RPCs for both `RPC_URL_1` & `RPC_URL_2`.
* To save: `CTRL`+`X`, `Y` & `ENTER`

### 5- Edit `.env`
```bash
nano .env
```
Replace the content of the file with the following codes:
```bash
ETH_PRIVATE_KEY=
ETH_PRIVATE_KEY2=
VPS_IP=
```
* Enter a value for `ETH_PRIVATE_KEY` & `ETH_PRIVATE_KEY2` (Operators Private key) & `VPS_IP`.
* To save: `CTRL`+`X`, `Y` & `ENTER`

### 6- Run Operators
Stop old docker node:
```bash
docker compose down -v
docker stop drosera-node
docker rm drosera-node
```

Run Operators:
```bash
docker compose up -d
```

### 7- Opt-in 2nd Operator
**Method 1**: Login with your 2nd Operator wallet in [Dashboard](https://app.drosera.io/), and Opt-in to your Trap

**Method 2**: via CLI
```bash
drosera-operator optin --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com --eth-private-key 2nd_Operator_Privatekey --trap-config-address Trap_Address
```
* Replace `2nd_Operator_Privatekey` & `Trap_Address`

### 8- Restart Operators
```bash
cd ~/Droseta-Network
docker compose up -d
```
* You will get Green Blocks after a while!

![image](https://github.com/user-attachments/assets/e639c5e9-cacd-42f4-8c4e-82597a6a71fd)


### 9- Useful Commands (If Running Two Operators)
Make sure you are in Operators directory:
```bash
cd ~/Drosera-Network
```

Restart Operators:
```bash
docker compose up -d
# OR
docker compose restart
```

Restart Operators seprately:
```bash
docker compose up -d drosera1
docker compose up -d drosera2
# OR
docker compose restart drosera1
docker compose restart drosera2
```

Kill Operators:
```bash
docker compose down -v
```

# Immortalize Discord username on-chain and Earn `Cadet` role!
Assuming your Trap is deployed and your operator is running, let's set up a new Trap to submit your Discord username on-chain and unlock an exclusive **Cadet** role

### 1. Create New Trap
1- Move to your trap directory:
```bash
cd my-drosera-trap
```
2- Create a new `Trap.sol` file:
```bash
nano src/Trap.sol
```
3- Paste the following contract code in it:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {ITrap} from "drosera-contracts/interfaces/ITrap.sol";

interface IMockResponse {
    function isActive() external view returns (bool);
}

contract Trap is ITrap {
    address public constant RESPONSE_CONTRACT = 0x4608Afa7f277C8E0BE232232265850d1cDeB600E;
    string constant discordName = "DISCORD_USERNAME"; // add your discord name here

    function collect() external view returns (bytes memory) {
        bool active = IMockResponse(RESPONSE_CONTRACT).isActive();
        return abi.encode(active, discordName);
    }

    function shouldRespond(bytes[] calldata data) external pure returns (bool, bytes memory) {
        // take the latest block data from collect
        (bool active, string memory name) = abi.decode(data[0], (bool, string));
        // will not run if the contract is not active or the discord name is not set
        if (!active || bytes(name).length == 0) {
            return (false, bytes(""));
        }

        return (true, abi.encode(name));
    }
}
```
* Replace `DISCORD_USERNAME` with your discord username.
* To save: `Ctrl+X`, `Y` & `Enter`

### 2. Edit `drosera.toml` config
```
nano drosera.toml
```
* Modify the values of the specified variables as follows:
* `path` = `"out/Trap.sol/Trap.json"`
* `response_contract` = `"0x4608Afa7f277C8E0BE232232265850d1cDeB600E"`
* `response_function` = `"respondWithDiscordName(string)"`

![image](https://github.com/user-attachments/assets/67b7cd71-0a01-49e7-aa69-540bd3d1f37d)

*  Your final `drosera.toml` file should match the example shown above.

### 3. Deploy Trap
1- Compile your Trap's Contract:
```
forge build
```
* If you got errors like: `command not found`, Enter `source /root/.bashrc` or reinstall dependecies from [here](https://github.com/0xmoei/Drosera-Network#1-configure-enviorments)

2- Test the trap before deploying:
```bash
drosera dryrun
```

3- Apply and Deploy the Trap:
```bash
DROSERA_PRIVATE_KEY=xxx drosera apply
```
* Replace `xxx` with your EVM wallet privatekey (Ensure it's funded with Holesky ETH)
* Enter the command, when prompted, write `ofc` and press `Enter`.

![image](https://github.com/user-attachments/assets/ddccd255-4288-4a8d-99a2-77ba1269089b)

### 4. Verify Trap can respond
After the trap is deployed, we can check if the user has responded by calling the `isResponder` function on the response contract.
```bash
source /root/.bashrc
cast call 0x4608Afa7f277C8E0BE232232265850d1cDeB600E "isResponder(address)(bool)" OWNER_ADDRESS --rpc-url https://ethereum-holesky-rpc.publicnode.com
```
* Replace `OWNER_ADDRESS` with your Trap's owner address. (Your main address that has deployed the Trap's contract)
* If you receive `true` as a response, it means you have successfully completed all the steps.

![image](https://github.com/user-attachments/assets/b6f89508-1ce4-46d6-8dcb-685ae7063d07)

* It may take a few minutes to successfully receive `true` as a response

### 5. Re-run Operator nodes
```
cd
cd Drosera-Network
```
```
docker compose up -d
```

### 6. View the List of submitted Discord Names
```bash
source /root/.bashrc
cast call 0x4608Afa7f277C8E0BE232232265850d1cDeB600E "getDiscordNamesBatch(uint256,uint256)(string[])" 0 2000 --rpc-url https://ethereum-holesky-rpc.publicnode.com/
```


# Troubleshooting Errors

## ⬜️⬜️ White Blocks for an Operator

![image](https://github.com/user-attachments/assets/7bf3bd34-d706-4c8a-8573-46d124258528)

* Use this `docker-compose.yaml` file instead.

```yaml
version: '3'
services:
  drosera1:
    image: ghcr.io/drosera-network/drosera-operator:latest
    container_name: drosera-node1
    network_mode: host
    volumes:
      - drosera_data1:/data
    command: node --db-file-path /data/drosera.db --network-p2p-port 31313 --server-port 31314 --eth-rpc-url RPC_URL_1 --eth-backup-rpc-url https://holesky.drpc.org --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 --eth-private-key ${ETH_PRIVATE_KEY} --listen-address 0.0.0.0 --network-external-p2p-address ${VPS_IP} --disable-dnr-confirmation true
    restart: always

  drosera2:
    image: ghcr.io/drosera-network/drosera-operator:latest
    container_name: drosera-node2
    network_mode: host
    volumes:
      - drosera_data2:/data
    command: node --db-file-path /data/drosera.db --network-p2p-port 31315 --server-port 31316 --eth-rpc-url RPC_URL_2 --eth-backup-rpc-url https://holesky.drpc.org --drosera-address 0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8 --eth-private-key ${ETH_PRIVATE_KEY2} --listen-address 0.0.0.0 --network-external-p2p-address ${VPS_IP} --disable-dnr-confirmation true
    restart: always

volumes:
  drosera_data1:
  drosera_data2:
```
* Replace `RPC_URL_1` & `RPC_URL_2` with your own Ethereum Holesky RPC, You can use [Alchemy](https://dashboard.alchemy.com/) or [QuickNode](https://dashboard.quicknode.com/) to create Ethereum Holesky RPC.
* You can use same RPCs for both `RPC_URL_1` & `RPC_URL_2`.
* To save: `CTRL`+`X`, `Y` & `ENTER`

**Re-run your Operators**:
```bash
cd ~/Drosera-Network
```
```bash
docker compose down -v
```
```bash
docker compose up -d
```

**Now Give it a few minutes. Boom!**

![image](https://github.com/user-attachments/assets/87a6eca8-5d77-44d6-84e8-f370200e02f3)



## 🚨 **Error**: Apply config failed
* You may get `Apply config failed` due to several issues:
![image](https://github.com/user-attachments/assets/326a931f-f107-4865-8d1b-add5a3735ce1)

* You need to verify if you have deployed any trap before.
* In your `drosera.toml` file, Verify that you have a line showing your trap's address as `address = "0x..."`

![image](https://github.com/user-attachments/assets/eb84d0c3-fb8d-43c5-aa00-c6e4422cebbd)

> If you don't see your trap's address as address = "0x..." line, Then follow the steps:

* Connect to the [Dashboard](https://app.drosera.io/) using same account you used to deploy your trap:

![image](https://github.com/user-attachments/assets/50b0b026-8a1d-492c-bb20-41ea1511e215)


* Select the **“Traps Owned”** button to display only traps that your account owns:

![image](https://github.com/user-attachments/assets/bc4550c6-b4a7-43ee-833f-fe179d9d5cf5)

* Click on the trap to go to the trap’s page:

![image](https://github.com/user-attachments/assets/e21666c7-fdc2-42f8-9ae9-7e5a5ca16267)

* On the trap’s page, click on the **“Trap Config”** address hyperlink. This will open a page to holesky etherscan.

![image](https://github.com/user-attachments/assets/09d1112c-8d7c-4d49-a4f2-01e660f01139)

* Click the button to copy the contract’s address:

![image](https://github.com/user-attachments/assets/9a8ecf32-a4f2-4da8-9b21-e3ddb2d977c8)

* Paste that address as the value for the corresponding trap’s address field in your drosera.toml file:

![image](https://github.com/user-attachments/assets/5934ce37-d144-4d0e-9084-8a789a6ebc01)
