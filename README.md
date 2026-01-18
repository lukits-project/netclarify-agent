# NetClarify Agent

Lightweight local agent for network diagnostics including real-time traceroute, ping testing, and network scanning.

## Features

| Feature | Description |
|---------|-------------|
| **Traceroute** | Real-time WebSocket-based network path tracing |
| **Ping** | ICMP ping tests with latency statistics |
| **Network Scan** | Discover devices on your local network |
| **Network Info** | View local network interface information |

## Download

See [Releases](https://github.com/lukits-project/netclarify-agent/releases) for all available downloads.

### Available Binaries

| Platform | Architecture | Binary |
|----------|--------------|--------|
| Linux | amd64 | `nc-agent-linux-amd64`, `netcly-linux-amd64` |
| Linux | arm64 | `nc-agent-linux-arm64`, `netcly-linux-arm64` |
| macOS | amd64 (Intel) | `nc-agent-darwin-amd64`, `netcly-darwin-amd64` |
| macOS | arm64 (Apple Silicon) | `nc-agent-darwin-arm64`, `netcly-darwin-arm64` |
| Windows | amd64 | `nc-agent-windows-amd64.exe`, `netcly-windows-amd64.exe` |

---

## Quick Install

### Linux/macOS

```bash
curl -sSL https://github.com/lukits-project/netclarify-agent/releases/latest/download/install.sh | sudo bash
```

### Windows (PowerShell as Administrator)

```powershell
irm https://github.com/lukits-project/netclarify-agent/releases/latest/download/install.ps1 | iex
```

---

## Verify Installation

```bash
curl http://127.0.0.1:43547/health
```

Expected response:
```json
{
  "status": "ok",
  "service": "netclarify-agent",
  "version": "X.Y.Z"
}
```

---

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

---

## API Endpoints

The agent runs on `http://127.0.0.1:43547` (localhost only).

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Agent health status |
| `/traceroute` | WebSocket | Real-time traceroute |
| `/ping` | GET | ICMP ping test |
| `/network-scan` | GET | Network device scan |
| `/network-info` | GET | Local network info |

---

## Requirements

- **Administrator/root privileges** required (ICMP needs raw sockets)
- Agent runs as a system service and starts automatically on boot

---

## License

NetClarify Agent is provided by [Lukits Enterprise Co., Ltd.](https://www.lukits.com)

Free for all users. See [NetClarify](https://netclarify.io) for more information.
