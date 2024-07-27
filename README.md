
# Running Cardano Node on Android Phone

This guide will help you run a Cardano node on your Android phone using Termux and Ubuntu. Follow these steps to get your node up and running.

## Prerequisites

Android phone with a lot of memory (minimum 12GB) and storage (minimum 200GB)

## Steps

### 1. Install Termux

- Download and install Termux from [F-Droid](https://f-droid.org/en/packages/com.termux/) or [GitHub](https://github.com/termux/termux-app).

### 2. Install Proot and Setup Ubuntu

- Open Termux and run the following commands to install proot and setup Ubuntu:

  ```bash
  pkg update && pkg upgrade
  pkg install proot proot-distro
  proot-distro install ubuntu
  proot-distro login ubuntu
  ```

### 3. Download Cardano-Node and Cardano-CLI for ARM64 Architecture

- Download the latest Cardano-node and Cardano-CLI binaries compiled for ARM64 from the Armada Alliance:

https://github.com/armada-alliance/cardano-node-binaries

### 4. Download Cardano Config Files and Genesis Files

- Download the Cardano configuration files and genesis files from the official Cardano documentation:

https://book.world.dev.cardano.org/environments/

### 5. Run the Node

- Create a script to run the Cardano node. Save the following script as `run-node.sh`:

  **Mainnet (`run-node.sh`):**

  ```bash
  #!/bin/bash
  /root/bin/cardano-node run \
      --topology /root/cnode/config/topology.json \
      --config /root/cnode/config/config.json \
      --database-path /root/cnode/db \
      --socket-path /root/cnode/db/node.socket \
      --port 3001
  ```

- Make the script executable:

  ```bash
  chmod +x run-node.sh
  ```

- Run the script to start the node:

  ```bash
  ./run-node.sh
  ```

### 6. Check if the Node is Working Properly

- Verify the node is running by checking the logs or using `cardano-cli` to query the node status:

  ```bash
  cardano-cli query tip --mainnet
  ```

### 7. Monitor Sync Progress

- Create a script named `check-sync.sh` to monitor the synchronization progress:

  ```bash
  #!/bin/bash

  # Path to the cardano-cli binary
  CARDANO_CLI="/root/bin/cardano-cli"

  # Path to the socket file
  CARDANO_NODE_SOCKET_PATH="/root/cnode/db/node.socket"

  # Get the current tip of the blockchain from the node
  currentTip=$($CARDANO_CLI query tip --mainnet --socket-path $CARDANO_NODE_SOCKET_PATH)

  # Extract the sync progress and block number
  syncProgress=$(echo $currentTip | jq -r '.syncProgress')
  blockNo=$(echo $currentTip | jq -r '.block')

  # Display the sync progress and block number
  echo "Current Sync Progress: $syncProgress%"
  echo "Current Block Number: $blockNo"

  # Optionally, display the full tip information
  echo "Full Tip Information:"
  echo $currentTip | jq
  ```

- Make the script executable:

  ```bash
  chmod +x check-sync.sh
  ```

- Run the script to check sync status:

  ```bash
  ./check-sync.sh
  ```

## Approximate Sync Times

- **Mainnet**: 24-48 hours

## Additional Notes

- Ensure that your phone has sufficient storage and is connected to a stable internet connection.
- Regularly update your Cardano binaries and configuration files to stay in sync with the network.

---

Feel free to modify and enhance this documentation based on your specific requirements. If you have any questions or need further assistance, let me know!
