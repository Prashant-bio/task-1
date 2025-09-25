# task-1
Nmap (Network Mapper)

## Overview

Nmap (Network Mapper) is an open-source tool for network discovery and security auditing. It finds live hosts, open ports, services (and versions), attempts OS detection, and runs scripted checks via the Nmap Scripting Engine (NSE).

> **Ethical note:** Only scan systems you own or have explicit permission to scan.

---

## Prerequisites

* Linux/macOS/Windows with Nmap installed.
* Root/Administrator for certain scans (SYN, UDP, OS detection).

Install (Ubuntu/Debian):

```bash
sudo apt update && sudo apt install -y nmap
```

---

## Scans performed (practical commands & quick interpretation)

Each command was executed against an authorized lab target (replace `TARGET` with IP/hostname).

* **Host discovery (ping scan)**

```bash
nmap -sn TARGET
```

*Purpose:* find live hosts.

* **SYN (half-open) scan**

```bash
sudo nmap -sS -p 1-200 -T4 TARGET
```

*Look for:* `SYN →` then `SYN/ACK ←` (open) or `RST ←` (closed).

* **TCP Connect scan**

```bash
nmap -sT -p 1-200 TARGET
```

*Full handshake visible; useful when root unavailable.*

* **UDP scan**

```bash
sudo nmap -sU -p 53,123,161 TARGET
```

*Watch for ICMP type 3 code 3 = port unreachable (closed) or no reply (possible open/filtered).*

* **Service & version detection**

```bash
nmap -sV TARGET
```

*Identifies service banners (SSH, HTTP, etc.).*

* **OS detection**

```bash
sudo nmap -O TARGET
```

*Attempts to fingerprint OS.*

* **Aggressive scan (combined detection)**

```bash
sudo nmap -A TARGET
```

*Runs OS + version + scripts + traceroute.*

* **All ports scan and save results**

```bash
sudo nmap -p- -sV -O -oA full_scan TARGET
```

*Scans 65k TCP ports and saves `.nmap`, `.xml`, `.gnmap`.*

* **NSE script example (HTTP enumeration)**

```bash
nmap -p 80,443 --script http-enum,http-title TARGET
```

* **Vulnerability scripts (authorized only)**

```bash
sudo nmap --script vuln TARGET
```



## Output & Artifacts

* Save Nmap outputs with `-oA name`.
* Save packet captures with Wireshark or `tcpdump -w scan.pcap`.
* Keep `.nmap` and `.pcap` for reporting and reproducibility.

---

## Safety & Reporting

* Only run NSE vuln scripts on authorized systems.
* Document scan times, targets and command lines used when reporting findings.

--
