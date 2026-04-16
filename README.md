# GoSpyder
> A lightweight, concurrent network reconnaissance tool built in Go.
![Go Version](https://img.shields.io/badge/Go-1.20%2B-00ADD8?style=flat-square&logo=go)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey?style=flat-square)
GoSpyder is a fast, modular network scanning tool that leverages Go's concurrency model (goroutines) to perform rapid host discovery, port scanning, service detection, and OS fingerprinting — all from a single interactive CLI.
---
Features
Module	Description
🔍 Host Discovery	Ping a subnet (CIDR) or single IP to identify live hosts
🔌 Port Scanning	Scan port ranges or specific ports via TCP connect
🛠️ Service Detection	Banner grabbing to identify FTP, HTTP, SSH, SMTP, MySQL and more
💻 OS Fingerprinting	TTL-based OS detection (Linux, Windows, Cisco, macOS, etc.)
🗺️ Full Recon	One-shot comprehensive scan combining all modules
---
Demo
```
██████╗  ██████╗ ███████╗██████╗ ██╗   ██╗██████╗ ███████╗██████╗
██╔════╝ ██╔═══██╗██╔════╝██╔══██╗╚██╗ ██╔╝██╔══██╗██╔════╝██╔══██╗
██║  ███╗██║   ██║███████╗██████╔╝  ╚████╔╝ ██║  ██║█████╗  ██████╔╝
██║   ██║██║   ██║╚════██║██╔═══╝    ╚██╔╝  ██║  ██║██╔══╝  ██╔══██╗
╚██████╔╝╚██████╔╝███████║██║         ██║   ██████╔╝███████╗██║  ██║
 ╚═════╝  ╚═════╝ ╚══════╝╚═╝         ╚═╝   ╚═════╝ ╚══════╝╚═╝  ╚═╝
                                              --- By Aman Sharma

Choose a Functionality
------------------------------------------------
[+] 1. Host Discovery
[+] 2. Port Scan
[+] 3. Service Detection
[+] 4. Operating System Detection
[+] 5. Full Recon
[+] 6. Exit
```
Sample Full Recon output:
```
[+] IP address 10.0.2.15 is Alive.

[+] 21 Port Open
[+] 80 Port Open
[+] 443 Port Open

[+] PORT 21 : FTP Service Detected
    Banner: 220 (vsFTPd 3.0.3)

[+] PORT 80 : HTTP Service Detected
    Banner: HTTP/1.0 200 OK | Server: SimpleHTTP/0.6 Python/3.11.8

[+] IP Address: 10.0.2.15 - Detected OS: Linux/FreeBSD/OSX (TTL: 64)
```
---
Installation
Prerequisites: Go 1.20 or later
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/gospyder.git
cd gospyder

# Build
go build -o gospyder main.go

# Run
./gospyder
```
> **Note:** Run with appropriate network privileges. On Linux/macOS, some features (e.g., ICMP ping) may require `sudo`.
---
Usage
GoSpyder presents an interactive menu on launch. Select a module by entering its number.
Host Discovery
```
Mode 1 — Ping a Subnet    : Enter CIDR (e.g. 192.168.1.0/24)
Mode 2 — Ping Single Host : Enter a single IP address
```
Port Scanning
```
Mode 1 — Range Scan    : Enter start and end port (e.g. 1–10000)
Mode 2 — Specified Ports: Enter individual ports one per line
```
Service Detection
```
Mode 1 — Range Scan    : Detects services across a port range
Mode 2 — Specified Ports: Targets specific ports for banner grabbing
```
Detected services: `HTTP`, `FTP`, `SSH`, `SMTP`, `MySQL`, and more.
OS Fingerprinting
Enter one or more IP addresses. GoSpyder retrieves TTL values and maps them to known OS families.
TTL	OS
64	Linux / FreeBSD / macOS
128	Windows
255	Cisco Device
254	Solaris / AIX
Full Recon
```
Mode 1 — Quick Scan   : Ports 1–10,000
Mode 2 — All Port Scan: Ports 1–65,535
```
Runs host discovery → port scan → service detection → OS fingerprinting in sequence. Supports multiple target IPs.
---
Architecture
GoSpyder is built around Go's concurrency primitives:
Goroutines — parallel scanning across multiple hosts and ports
sync.WaitGroup — synchronizes concurrent scan completion
sync.Mutex — prevents interleaved console output from concurrent goroutines
net.DialTimeout — connection attempts with configurable timeouts to avoid hangs
Module Overview
```
gospyder/
├── main.go            # Entry point, interactive menu loop
├── host_discovery.go  # pingIP(), modifyIP()
├── port_scan.go       # scanPorts(), scan_Specified_Ports()
├── service_detect.go  # detectService(), readBanner(), sendHTTPRequest()
│                        Multi_IP_serviceDetection(), Multi_port_serviceDetection()
├── os_detect.go       # getTTL(), detectOS()
└── full_recon.go      # Orchestrates all modules in sequence
```
---
Comparison with Nmap
Feature	GoSpyder	Nmap
Ease of Use	Interactive prompts, beginner-friendly	Command-line flags, steeper learning curve
Multiple Host Input	Native multi-host support in one session	Requires scripting or range notation
Full Recon	Single command, all modules	Multiple separate commands
Installation	Single binary, minimal dependencies	Larger install with additional libs
Customization	Targeted use-case focused	Highly scriptable (NSE), expert-oriented
Output	Structured, color-coded	Detailed but dense
GoSpyder is not a Nmap replacement — it is a focused, lightweight alternative optimized for ease of use and rapid reconnaissance.
---
Limitations & Known Issues
OS detection relies solely on TTL values; accuracy may vary with TTL manipulation or proxies
Service detection uses banner grabbing — services that don't emit banners will show "No Banner received"
Host discovery uses ICMP ping via system `ping` command; hosts blocking ICMP will appear offline
Not designed for stealth scanning; IDS/firewalls may detect and block scans
---
Future Work
[ ] Deep service version detection (beyond banner grabbing)
[ ] CVE cross-referencing for detected services
[ ] Stealth scanning modes (slow-rate, fragmented packets)
[ ] JSON / CSV export of scan results
[ ] Web-based dashboard for scan visualization
[ ] Real-time alerts via Slack / email integration
---
Disclaimer
GoSpyder is intended for authorized security testing and educational purposes only. Scanning networks or hosts without explicit permission is illegal and unethical. The author is not responsible for any misuse of this tool.
Always obtain proper authorization before scanning any network or system.
---
Author
Aman Sharma — Cybersecurity Researcher & Penetration Tester
![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat-square&logo=linkedin)
![Blog](https://img.shields.io/badge/Blog-AshSec.blog-FF5722?style=flat-square)
