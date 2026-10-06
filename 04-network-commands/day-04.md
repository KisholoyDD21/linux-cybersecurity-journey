# Day 4 — Network Commands

## 1. Interface Configuration

### ifconfig

Displays and configures wired network interfaces.

```bash
ifconfig
```

> Note: `ifconfig` is deprecated on modern Linux. Prefer `ip addr` or `ip link`.

---

### iwconfig

Displays and configures wireless network interfaces.

```bash
iwconfig
```

---

## 2. Ping — Connectivity Testing

The `ping` command sends ICMP echo requests to test network connectivity.

```bash
ping <target-ip>
```

### Example: Successful Ping

```bash
┌──(root㉿kali)-[~]
└─# ping 192.168.67.128
PING 192.168.67.128 (192.168.67.128) 56(84) bytes of data.
64 bytes from 192.168.67.128: icmp_seq=1 ttl=64 time=0.167 ms
64 bytes from 192.168.67.128: icmp_seq=2 ttl=64 time=0.068 ms
64 bytes from 192.168.67.128: icmp_seq=3 ttl=64 time=0.076 ms
64 bytes from 192.168.67.128: icmp_seq=4 ttl=64 time=0.070 ms
64 bytes from 192.168.67.128: icmp_seq=5 ttl=64 time=0.078 ms
64 bytes from 192.168.67.128: icmp_seq=6 ttl=64 time=0.094 ms
64 bytes from 192.168.67.128: icmp_seq=7 ttl=64 time=0.072 ms
64 bytes from 192.168.67.128: icmp_seq=8 ttl=64 time=0.075 ms
64 bytes from 192.168.67.128: icmp_seq=9 ttl=64 time=0.066 ms
64 bytes from 192.168.67.128: icmp_seq=10 ttl=64 time=0.057 ms
64 bytes from 192.168.67.128: icmp_seq=11 ttl=64 time=0.066 ms
^C
--- 192.168.67.128 ping statistics ---
11 packets transmitted, 11 received, 0% packet loss, time 10237ms
rtt min/avg/max/mdev = 0.057/0.080/0.167/0.028 ms
```

**Key observations:**
- The target machine responds (ICMP echo replies received)
- 0% packet loss indicates good connectivity
- `Ctrl+C` stops the continuous ping

### Example: Failed Ping

```bash
┌──(root㉿kali)-[~]
└─# ping 192.168.6.128
PING 192.168.6.128 (192.168.6.128) 56(84) bytes of data.
^C
--- 192.168.6.128 ping statistics ---
16 packets transmitted, 0 received, 100% packet loss, time 15363ms
```

**Key observations:**
- 100% packet loss — the target is not responding
- Could mean: host is down, firewall blocking ICMP, or wrong subnet

---

## 3. netstat — Active Connections

Shows network connections, routing tables, and interface statistics.

```bash
netstat -ano
```

**Flags:**
- `-a` — Show all connections (listening and established)
- `-n` — Show numerical addresses (no DNS resolution)
- `-o` — Show timers (on Linux, shows timer info for TCP)

### Example Output

```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       Timer
udp        0      0 192.168.67.128:68       192.168.67.254:67       ESTABLISHED off (0.00/0/0)
raw6       0      0 :::58                   :::*                    7           off (0.00/0/0)

Active UNIX domain sockets (servers and established)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  3      [ ]         STREAM     CONNECTED     17825    /run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     19583
... (many UNIX socket connections)
```

> Note: `netstat` is deprecated. Modern replacement: `ss` (socket statistics).

---

## 4. ARP — Address Resolution Protocol

### arp -a

Displays the ARP cache (IPv4 to MAC address mappings for local network).

```bash
arp -a
```

**Breakdown:**
- `arp` — Address Resolution Protocol utility
- `-a` — Display all current ARP entries

### Example Output

```
Interface: 192.168.1.10
Internet Address      Physical Address      Type
192.168.1.1           AA-BB-CC-11-22-33     dynamic
192.168.1.15          44-55-66-77-88-99     dynamic
192.168.1.20          12-34-56-78-9A-BC     dynamic
```

**What this means:**
- `192.168.1.1` → `AA-BB-CC-11-22-33` (typically the router)
- Your computer knows the MAC address for these IPs
- Communication on Ethernet/Wi-Fi requires MAC addresses

### Cybersecurity Relevance

`arp -a` gives a quick view of devices your machine has recently communicated with on the local network — useful for network reconnaissance.

> Limitation: Only shows entries currently in ARP cache; not a complete network scanner.

### Modern Alternative: ip neigh

```bash
ip neigh
```

This is the modern `iproute2` replacement for `arp -a`.

---

## 5. Routing Table

### route (legacy)

Displays the kernel IP routing table.

```bash
route
```

### Example Output

```
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         192.168.1.1     0.0.0.0         UG    100    0        0 wlan0
192.168.1.0     0.0.0.0         255.255.255.0   U     100    0        0 wlan0
```

**Column Meanings:**

| Column | Meaning |
|--------|---------|
| Destination | Network the packet is trying to reach |
| Gateway | Router to send it through |
| Genmask | Defines the size/range of that network |
| Flags | Information about the route |
| Iface | Network interface used |

### Flag Meanings

| Flag | Meaning |
|------|---------|
| U | Route is Up/active |
| G | Route uses a Gateway |

**Example interpretation:**
- `default → 192.168.1.1 → UG` — Internet traffic goes to gateway 192.168.1.1
- `192.168.1.0 → U` — Local network directly connected, no gateway needed

### Modern Alternative: ip route

```bash
ip route
```

### Example Output

```
default via 192.168.1.1 dev wlan0
192.168.1.0/24 dev wlan0 proto kernel scope link
```

---

## 6. Detailed Routing Table Analysis

### Example with Full Columns

```
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         192.168.67.2    0.0.0.0         UG    100    0        0 eth0
192.168.67.0    0.0.0.0         255.255.255.0   U     100    0        0 eth0
```

### Field Breakdown

#### 1. Flags
- **UG** — Route is active (U) and uses a Gateway (G)
- **U** — Route is active, directly connected (no gateway)

#### 2. Metric
- Cost/preference of the route
- Lower metric = more preferred
- If multiple routes exist to same destination, lower metric wins

#### 3. Ref (Reference Count)
- Historically: active references using the route
- Modern Linux: generally 0, not useful for troubleshooting

#### 4. Use
- Historically: count of packets routed via this route
- Often not maintained reliably; focus on other fields

#### 5. Iface (Interface)
- Network interface traffic leaves through
- Example: `eth0`, `wlan0`

### Putting It Together

**Line 1:** `default 192.168.67.2 0.0.0.0 UG 100 0 0 eth0`
- For any destination without a specific route → send to gateway 192.168.67.2 via eth0
- Route is active (U), uses gateway (G), metric 100

**Line 2:** `192.168.67.0 0.0.0.0 255.255.255.0 U 100 0 0 eth0`
- 192.168.67.0/24 network directly connected via eth0
- No gateway needed (no G flag)

### Mental Model: Routing + ARP

```
IP destination
    ↓
route / ip route    → "What path should I take?"
    ↓
Gateway
    ↓
ARP / ip neigh      → "What MAC address for that next-hop IP?"
    ↓
Ethernet/Wi-Fi frame
```

---

## 7. Commands I Practiced

| Command | Description |
|---------|-------------|
| `ifconfig` | Show/configure wired interfaces (legacy) |
| `iwconfig` | Show/configure wireless interfaces (legacy) |
| `ip addr` | Modern interface configuration |
| `ip link` | Modern link-layer configuration |
| `ping <ip>` | Test connectivity with ICMP |
| `netstat -ano` | Show all connections with timers (legacy) |
| `ss -tuln` | Modern socket statistics |
| `arp -a` | Show ARP cache (legacy) |
| `ip neigh` | Modern neighbor/ARP table |
| `route` | Show routing table (legacy) |
| `ip route` | Modern routing table |

---

## What I Learned Today

- Network interface tools: `ifconfig`/`iwconfig` (legacy) vs `ip addr`/`ip link` (modern)
- `ping` for connectivity testing — ICMP echo request/reply
- Reading ping output: packet loss, RTT, TTL
- `netstat -ano` for connection analysis (legacy) → `ss` modern replacement
- ARP cache with `arp -a` — maps IP to MAC on local network
- Modern `ip neigh` for neighbor discovery
- Routing table: `route` (legacy) vs `ip route` (modern)
- Routing flags: `U` (up), `G` (gateway)
- Metric for route preference (lower = preferred)
- Interface (`Iface`) determines physical path
- Layer 3 (routing) + Layer 2 (ARP) work together for packet delivery