# 📂 Explaining the Repository

This repository provides a complete Self-Sovereign Identity (SSI) demo setup using ACA-Py, including a running agent, a backend server, and a frontend interface. The folder structure is as follows:

```
/
├── acapy/            # ACA-Py agent cloned from OpenWallets (have to clone separately)
├── ssi-tutorial/
│ ├── demo/           # Contains the server and supporting files
│ │ ├── acapy/        # Backend server setup (We will be using acapy agent)
│ │ └── credo/
│ └── interface/      # Frontend interface (React-based)
```

- **`acapy/`**: This is the official ACA-Py agent cloned from OpenWallet. It's used to run SSI agents for credential issuance, verification, and communication.
- **`ssi-tutorial/demo/acapy`**: Acts as the backend server, issuing and verifying credentials.
- **`ssi-tutorial/interface`**: A web interface to interact with the agent.

---

## 📘 What is ACA-Py?

ACA-Py (Aries Cloud Agent Python) is a Python-based framework for building SSI agents. It offers:

- 🔐 Secure credential management  
- 🤝 Agent-to-agent DIDComm communication  
- 📜 Verifiable credential issuance and verification  
- 🎯 Hyperledger Indy support  
- 🔄 Out-of-the-box interoperability  

---

## ⚙️ Prerequisites

Make sure the following tools are installed on your system before continuing:

> 📌 *Use versions equal or higher than those listed below.*

- Docker (>= v24.0.1)  
- Node.js (>= v16)  
- Python (>= 3.12)  
- Yarn (>= v1.22.22)  
- Git  
- Ngrok  

---
# 🔨 Setup Guide

The setup guide will be easy and straightforward, as most of the work is complete. You just need to install the requirements and setup the environment files.
We will complete the setup in 5 steps:

- Clone the Repository
- ACA-Py Agent Setup
- Server Setup
- Interface Setup
- Mobile Wallet Setup

## 🪢 Step I - Clone this Repository (This will be your `root` folder)
```bash
git clone https://github.com/mutasinjawad/SSI.git

cd SSI
```
## 🧪 Step II - ACA-Py Agent Setup (`acapy/` folder)

> *You need to clone the ACA-Py repo separately. It will be in the same directory as `ssi-tutorial/`*

### Clone OpenWallets official ACA-Py repository

```
# Clone OpenWallets official ACA-Py repository
git clone -b 0.12.3 https://github.com/openwallet-foundation/acapy.git

# Go to the demo directory
cd acapy/demo

# Update asyncpg version to avoid compatibility issues
sed -i 's/asyncpg.*/asyncpg~=0.28.0/' requirements.txt
```

### Install Dependencies

```
# Install Python dependencies
python3 -m pip install -r requirements.txt

# For Python >= 3.12 on Debian-based systems (e.g., Ubuntu):
python3 -m pip install -r requirements.txt --break-system-packages
```

⚠️ Important: Keep Docker running in the background. <br/>
🐧 Use Git Bash or a Linux-based CLI (like WSL or Ubuntu terminal) as ./run_demo is a Unix shell script.

### Start Ngrok
```bash
ngrok http 8020
```
> *This gives you a public HTTPS URL — you’ll use it in the next step.*

### Start the Agent
```bash
# This will start the ACA-Py agent and attach it to the ngrok endpoint
LEDGER_URL=http://dev.greenlight.bcovrin.vonx.io ./run_demo faber

# or you can set a manual endpoint yourself:
LEDGER_URL=http://dev.greenlight.bcovrin.vonx.io AGENT_ENDPOINT=https://{ngrok_url} ./run_demo faber
# You can find the ngrok_url in the terminal where you started it
# The line is like this:
# Forwarding     https://{ngrok_url} -> http://localhost:8020  
```
> *Troubleshooting tip: If the above fails with a network error, try switching to mobile data or a different Wi-Fi.*

## 🧪 Step III - Server Setup (ssi-tutorial/demo/acapy)
The server acts as a middleware between the cloud agents and the interface.

```bash
cd ssi-tutorial/demo/acapy

# Copy the sample .env file
cp .env.sample .env
# Edit `.env` with the correct variables (e.g., ACAPY_ADMIN_URL and {your-ip-address})

# Install dependencies
yarn install

# Start as an issuer or verifier
yarn issuer
# or
yarn verifier
```

## 🧪 Step IV - Interface Setup (ssi-tutorial/interface)
The frontend interface will help to establish a connection between the cloud agent and the mobile wallet.

```bash
cd ssi-tutorial/interface

# Copy the sample .env file
cp .env.sample .env
# Update API URL in `.env`
# For API v2 (recommended):
NEXT_PUBLIC_API_URL=http://{your_ip_address}:4000/v2

# Install dependencies
yarn install

# Start the development server
yarn dev
```

## 📲 Step V - Setup Mobile Wallet

Download the Bifold app to connect with your ACA-Py agent:

[`Click here to download`](https://drive.google.com/uc?export=download&id=10Qv5FNXOsp6-kyafJefXYYSe_v5bpfuq)

After installing:

* Create a 6-digit PIN to log in
* You can:

  * Connect with issuers/verifiers
  * Send message to other parties
  * Store credentials
  * Present proofs
  * Make your own invitation QR Code and share it with other parties
  * And more 🚀 (Explore Yourself)

--- 
# 🏎️ Usage Guide
<div style="border-left: 4px solid #28a745; padding-left: 20px; margin: 15px 0;">

#### 1. 🔗 Establish a Connection

- Open the **issuer interface** and generate a connection invitation.
- Use the **Bifold mobile wallet** to scan the QR code and establish a secure DID connection.

#### 2. 📜 Receive a Credential

- The issuer will send a credential offer to your connected wallet.

#### 3. ✅ Accept the Credential

- Open the Bifold wallet, review the credential offer, and accept it to store the credential securely.

#### 4. 🔍 Present the Credential

- The issuer can also act as a **verifier**, requesting proof of the issued credential.
- You’ll receive a proof request in your Bifold wallet — share the requested credential in response.

Alternatively, if using a separate verifier:

- Generate a connection invitation from the **verifier agent**.
- Scan the QR code with the Bifold wallet to establish a new connection.
- The verifier sends a proof request.
- Share the credential proof using your wallet.
- The verifier will validate the shared proof.
</div>

# 📽️ For video demonstration
> You can go [`Here`](https://youtu.be/gqjBc5zikMs)
