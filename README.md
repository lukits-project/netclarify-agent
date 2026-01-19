# NetClarify Agent

Lightweight local agent for network diagnostics including traceroute, ping, and network scanning.

## Quick Start

```bash
# Build
cd agent
go build -o netclarify-agent ./cmd/server
go build -o netcly ./cmd/netcly

# Install (macOS)
sudo ./install.sh

# Verify
curl http://127.0.0.1:43547/health
```

## Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Agent health status |
| `/traceroute` | WebSocket | Real-time traceroute |
| `/ping` | GET | ICMP ping test |
| `/network-scan` | GET | Network device scan |
| `/network-info` | GET | Local network info |

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

Copyright © 2025 NetClarify
