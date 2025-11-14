# Grin Node for Umbrel

Run a full **Mimblewimble/Grin node or Grin++ node** with a powerful, modern dashboard directly on your Umbrel OS.  
This app bundles a Rust-based Grin node, a controller backend, and a full web UI for monitoring everything in real time.

---

## Features

- **Full Rust Grin Node (v5.4+)**
- **Full Grin++ Node**
- **Live Dashboard UI**
  - Chain height & sync progress
  - Connected peers (with geolocation map)
  - Mempool transactions
  - Block explorer (inputs/outputs/kernels)
  - Node health & status
- **Peer Management**
  - Ban/unban peers
  - Live peer performance metrics
- **Storage Persistence**
  - All chain data stored under Umbrel’s app directory
- **Automatic Startup**
  - Node restarts automatically with Umbrel
- **Two-container architecture**
  - `ui` → Nginx-hosted web interface  
  - `controller` → Rust-based backend + Grin node

---

## Architecture Overview

```
Umbrel Reverse Proxy
        │
        ▼
   app_proxy  ──────►  ui container (Ngininx, Port 80)
                          │
                          ▼
                controller container (Grin backend)
```

- **UI image:** `wiesche89/grin-node-docker:0.1.0`  
- **Controller image:** `wiesche89/grin-node-controller:0.1.0`

---

## Installation (via Community App Store)

1. Open Umbrel → App Store  
2. Click the **three dots** in the top right  
3. Select **Community App Stores**  
4. Add this store URL:

```
https://github.com/wiesche89/umbrel-community-app-store
```

5. The **Grin Node** app will appear under your custom store  
6. Click **Install**

Umbrel will deploy:
- The UI frontend  
- The controller backend  
- The Rust Grin node  
- The Grin++ node 
- Persistent volumes for chain + config

---

## Data Persistence

All blockchain and config data is stored persistently inside Umbrel:

```
<umbrel>/app-data/grin-node/
    ├── grin/
    │   ├── data/       ← chain data (rust node)
    │   └── config/     ← node configuration
```

This means:

- Updating the app **does not delete the blockchain**
- Removing & reinstalling the app **keeps your chain + config**

---

## Ports

External ports exposed by the controller container:

| Port | Purpose            |
|------|--------------------|
| 3414 | Grin P2P           |
| 3413 | Foreign API        |
| 3415 | Owner API          |

The UI is routed internally through Umbrel’s reverse proxy and does not expose ports directly.

---

## Accessing the Dashboard

After installation, open:

```
http://umbrel.local/grin-node
```

or via your Umbrel device IP:

```
http://<umbrel-ip>/grin-node
```

---

## Troubleshooting

### UI shows “Cannot connect to controller”
Check whether the controller container is running:

```
docker ps | grep grin-node_controller
```

Restart the app from the Umbrel UI if needed.

### Node stuck on "Synchronizing"
Initial block download may take time depending on SSD speed and internet.

### Ports already in use
If another app uses 3414/3413/3415, installation may fail.  
Stop the conflicting app, or modify ports manually.

---

## Developer Info

This app uses two Docker images:

### UI
- Docker Hub:  
  https://hub.docker.com/repository/docker/wiesche89/grin-node-docker

### Controller + Grin Node
- Docker Hub:  
  https://hub.docker.com/repository/docker/wiesche89/grin-node-controller

Pullable tags:

```
wiesche89/grin-node-docker:0.1.0
wiesche89/grin-node-controller:0.1.0
```
---

## Credits

- **Mimblewimble / Grin Developers** — https://github.com/mimblewimble/grin  
- **Grin++ Developers** — https://github.com/GrinPlusPlus/GrinPlusPlus
- **Umbrel Team** — Community App Store framework  
- **wiesche89** — Grin Node Controller, UI, packaging
  - https://github.com/wiesche89/grin-node-docker
  - https://github.com/wiesche89/grin-node-controller
  - https://github.com/wiesche89/GrinPlusPlus
  - https://github.com/wiesche89/grin

---

## 📜 License

MIT License  
See `LICENSE` file for details.
