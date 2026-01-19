# NetClarify Agent

Lightweight local agent for network diagnostics including real-time traceroute, ping, and network scanning.

## Features

| Feature | Description |
|---------|-------------|
| **Traceroute** | Real-time WebSocket-based network path tracing |
| **Ping** | ICMP ping tests with latency statistics |
| **Network Scan** | Discover devices on your local network |
| **Network Info** | View local network interface information |

## Server-Side Integration

The NetClarify Backend utilizes this agent to perform "Server-Side" diagnostics.
- **Default Port:** `43547` (HTTP/WebSocket)
- **Binder:** Listens on `0.0.0.0` by default to allow connections from Docker containers.
- **CORS:** Allowed for all origins to facilitate local development and container networking.

If running NetClarify in **Docker**, ensure the Backend can reach the Host Agent:
1. Agent must be running on the Host.
2. Backend `.env` must be configured with `SERVER_AGENT_URL=http://YOUR_HOST_IP:43547`.

## Download

See [Releases](../../releases) for all available downloads.

### Available Binaries

| Platform | Architecture | Binary |
|----------|--------------|--------|
| Linux | amd64 | `nc-agent-linux-amd64`, `netcly-linux-amd64` |
| Linux | arm64 | `nc-agent-linux-arm64`, `netcly-linux-arm64` |
| macOS | amd64 (Intel) | `nc-agent-darwin-amd64`, `netcly-darwin-amd64` |
| macOS | arm64 (Apple Silicon) | `nc-agent-darwin-arm64`, `netcly-darwin-arm64` |
| Windows | amd64 | `nc-agent-windows-amd64.exe`, `netcly-windows-amd64.exe` |

## Quick Install

### Linux/macOS
```bash
curl -sSL https://github.com/lukits-projects/netclarify-agent/releases/latest/download/install.sh | sudo bash
```

### Windows (PowerShell as Administrator)
```powershell
irm https://github.com/lukits-projects/netclarify-agent/releases/latest/download/install.ps1 | iex
```

> **💡 Auto-Configuration:** The installer will automatically detect your host IP and offer to update your NetClarify `.env` file with `SERVER_AGENT_URL` if found.

### Verify Installation
```bash
curl http://127.0.0.1:43547/health
```

**Expected response:**
```json
{
  "status": "ok",
  "service": "netclarify-agent",
  "version": "X.Y.Z"
}
```

## Uninstall

### Windows (PowerShell as Admin)
```powershell
sc stop NetClarifyAgent
sc delete NetClarifyAgent
Remove-Item -Recurse -Force "$env:ProgramFiles\NetClarify"
```

### macOS
```bash
sudo launchctl unload /Library/LaunchDaemons/com.netclarify.agent.plist
sudo rm /Library/LaunchDaemons/com.netclarify.agent.plist
sudo rm /usr/local/bin/netclarify-agent /usr/local/bin/netcly
```

### Linux
```bash
sudo systemctl stop netclarify-agent
sudo systemctl disable netclarify-agent
sudo rm /etc/systemd/system/netclarify-agent.service
sudo rm /usr/local/bin/netclarify-agent /usr/local/bin/netcly
sudo systemctl daemon-reload
```

## Build

```bash
cd agent
go build -o netclarify-agent ./cmd/server
go build -o netcly ./cmd/netcly

# Install (macOS)
sudo ./install.sh

# Verify
curl http://127.0.0.1:43547/health
```

## API Endpoints

The agent runs on `http://127.0.0.1:43547` (localhost only by default, or 0.0.0.0 if configured).

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Agent health status |
| `/traceroute` | WebSocket | Real-time traceroute |
| `/ping` | GET | ICMP ping test |
| `/network-scan` | GET | Network device scan |
| `/network-info` | GET | Local network info |

## Requirements

- Administrator/root privileges required (ICMP needs raw sockets)
- Agent runs as a system service and starts automatically on boot

## Commands

```bash
# Update agent
sudo ./update.sh

# Verify installation
./verify.sh

# Check health
curl http://127.0.0.1:43547/health

# View logs (macOS)
tail -f /var/log/netclarify-agent.log

# Restart agent (macOS)
sudo launchctl unload /Library/LaunchDaemons/com.netclarify.agent.plist
sudo launchctl load /Library/LaunchDaemons/com.netclarify.agent.plist
```

## NetCly CLI

```bash
# Simple trace
sudo ./netcly 1.1.1.1

# With options
sudo ./netcly google.com --count 20 --json
```

## Documentation

See the main documentation at [`../docs/07-agent.md`](../docs/07-agent.md) for:
- Complete installation guide
- All command options
- Troubleshooting
- Cross-platform builds

## License

NetClarify Agent is provided by Lukits Enterprise Co., Ltd.  

Copyright © 2025 NetClarify
