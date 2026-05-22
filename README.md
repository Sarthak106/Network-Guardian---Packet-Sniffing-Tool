# Network Guardian 🛡️
**A Python-based real-time packet sniffer with anomaly detection**

> Built by Sarthak Khaiwal | [GitHub](https://github.com/Sarthak106)

---

## Features

| Feature | Details |
|---|---|
| **Real-time capture** | Captures live TCP, UDP, ICMP, ARP, DNS packets |
| **Protocol filtering** | Filter by protocol via CLI flag |
| **Anomaly detection** | Port scan, ICMP flood, NULL scan, XMAS scan |
| **Suspicious ports** | Flags traffic to SSH, RDP, SMB, Metasploit ports |
| **Live statistics** | Protocol breakdown, top IPs, packets/sec |
| **JSON logging** | All packets and alerts saved in JSONL format |
| **Hex payload dump** | Verbose mode shows raw packet payload |

---

## Project Structure

```
network_guardian/
├── main.py          # CLI entry point (argparse)
├── sniffer.py       # Core capture engine (Scapy)
├── analyzer.py      # Packet analysis + anomaly detection
├── logger.py        # JSON file logging
├── display.py       # Colored terminal output
├── requirements.txt
└── README.md
```

---

## Setup

```bash
# 1. Clone the repo
git clone https://github.com/Sarthak106/network-guardian
cd network-guardian

# 2. Install dependencies (Python 3.10+)
pip install -r requirements.txt

# 3. Run (requires root for raw socket access)
sudo python main.py
```

---

## Usage Examples

```bash
# Capture all traffic (unlimited)
sudo python main.py

# Capture only TCP on interface eth0
sudo python main.py -i eth0 -f tcp

# Capture 200 packets and save to log
sudo python main.py -c 200 -o session.json

# Enable anomaly/port-scan detection
sudo python main.py --detect-anomalies

# Verbose mode: show raw payload hex dump
sudo python main.py --verbose

# Full options
sudo python main.py -i wlan0 -f tcp -c 500 -o capture.json --detect-anomalies --verbose
```

---

## Anomaly Detection

Network Guardian detects the following attack patterns:

| Detection | Trigger |
|---|---|
| **Port Scan** | One IP hits ≥ 15 unique ports within 60 seconds |
| **ICMP Flood** | One IP sends > 20 ICMP packets within 5 seconds |
| **NULL Scan** | TCP packet with zero flags (stealth OS fingerprinting) |
| **XMAS Scan** | TCP packet with FIN + PSH + URG set (Nmap evasion) |
| **Suspicious Ports** | Traffic to SSH, Telnet, RDP, SMB, Metasploit, Redis, etc. |

---

## Sample Output

```
20:14:33  TCP     192.168.1.5:54321     → 142.250.80.46:443       1420 B [SYN ACK]
20:14:33  UDP     192.168.1.5:55302     → 8.8.8.8:53               78 B
20:14:34  ICMP    192.168.1.1           → 192.168.1.5              84 B [type=0 code=0]

⚠  ALERT  ⚠   PORT SCAN DETECTED | Source: 10.0.0.23 hit 17 unique ports in 60s
```

---

## Log Format (JSONL)

Each line in the output file is a valid JSON object:
```json
{"type": "PACKET", "timestamp": "20:14:33", "protocol": "TCP", "src_ip": "192.168.1.5", "dst_ip": "142.250.80.46", "src_port": 54321, "dst_port": 443, "length": 1420, "flags": "SYN ACK", "payload_hex": null}
{"type": "ALERT",  "timestamp": "20:14:40", "message": "PORT SCAN DETECTED | Source: 10.0.0.23 hit 17 unique ports in 60s", "src_ip": "10.0.0.23"}
```

---

## Skills Demonstrated

- **Python** — OOP, threading, file I/O, argparse, ANSI formatting
- **Scapy** — raw socket capture, BPF filters, packet layer inspection
- **Network Protocols** — TCP/IP, UDP, ICMP, ARP, DNS
- **Security Concepts** — port scanning, ICMP floods, NULL/XMAS scans
- **Software Design** — modular architecture, separation of concerns

---

## Requirements

- Python 3.10+
- Root / Administrator privileges (raw socket access)
- Linux or macOS recommended (Kali Linux ideal)
- `scapy==2.5.0`
