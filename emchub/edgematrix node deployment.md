# EMC Node Deployment

## Requirements
The EdgeMatrixComputing network currently prioritizes GPU providers from EPN (EdgeMatrix Partner Network) partners.  

- **If you represent an IDC or computing center**, please consider joining the EPN program.  
- **If you are an individual node operator capable of providing stable computing power for AI applications and clients**, deploying/staking tokens to your node to join the edgematrix network. Contact administrators on Discord for details.  

---

## System Requirements
- **OS**: Ubuntu 22.04  
- **GPU Driver**: Version 535, VRAM >=24G  

---

## 1. Install Docker and NVIDIA Virtualization
```bash
# 1. . Install Docker
sudo apt update
curl -fsSL https://get.docker.com -o get-docker.sh && chmod +x get-docker.sh && bash get-docker.sh

# 2. Install NVIDIA GPU Docker Virtualization
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

# 3. Verify deployment
sudo docker run --rm --gpus=all nvidia/cuda:11.8.0-base-ubuntu22.04 nvidia-smi
```

## 2. Deploy EMC and EMC Cloud
```bash
curl -s https://install.edgematrix.pro/setup_linux.sh | sh
curl -s https://install.edgematrix.pro/setup_cloud_linux.sh | sh

cd emc
./start.sh

cd ../cloud
./cloud_client.sh
```

## 3. Register Node Reward Wallet
Replace "xxxxxx" with your Arbitrum chain wallet address
```bash
./edge-matrix-computing node register --commit set --node computing --owner xxxxxx
```

## 4. Node Wallet Binding and Staking
```bash
After registration, you must:

Bind your wallet to the node

Stake EMC tokens to receive task rewards

Important Notes:

Confirm node whitelist details with official staff on Discord before staking

Staking URL: https://dashboard.emc.network/nodes/xxxxxx
(Replace xxxxxx with your Node ID, visible during EMC CLOUD startup or via verification scripts in Section 5)
```

## 5. Verification and Monitoring Scripts
Check version,Query Node ID,Check node status
```bash
./edge-matrix-computing version

./edge-matrix-computing secrets output --data-dir edge_data

./edge-matrix-computing node status
```
Node Type Clarification:

Validator/Routing Nodes: Require EMC token staking (Contact us for registration)

Computing Nodes: Recommended for most users

## Reinstallation Guide
Critical: Backup emc/edge_data to retain original Node ID and wallet address.
1. Terminate processes
2. Backup data
3. Clean installation
4. Reinstall EMC node
5. Restore data
```bash
sudo pkill edge-matrix 
sudo pkill nomad

cd emc
sudo cp -r edge_data ../

cd ..
sudo rm -rf emc cloud emc_cloud_linux_64.tgz emc_linux_64.tgz

sudo cp -rf edge_data emc/
```

For support, contact us on [Telegram](https://t.me/emc_network) or [Discord](https://discord.com/invite/emcnetwork).