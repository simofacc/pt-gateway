# SRIJ VPN Tunnel Architecture - Simplified with Full Control

## Overview

This document provides a **simplified, operator-controlled architecture** for the SRIJ site-to-site VPN tunnel, giving you complete control over VPN configuration (Phase 1, Phase 2, etc.) without requiring constant involvement from your hosting provider.

**Key Insight**: SRIJ provides IPsec VPN parameters (Phase 1 and Phase 2 settings) for a site-to-site tunnel. You need a VPN gateway appliance that **you control** to terminate this tunnel.

---

## Problem Statement

**Original Challenge**:
- SRIJ connects via site-to-site IPsec VPN tunnel
- SRIJ provides Phase 1 and Phase 2 parameters
- You need to configure and maintain this tunnel
- Relying on hosting provider for VPN changes = slow, costly, inflexible

**Solution**:
Deploy a **dedicated VPN/firewall appliance** that you fully control, sitting at the edge of your infrastructure.

---

## Simplified Architecture with VPN Control

### High-Level Diagram

```
                         Internet
                            │
                            │
            ┌───────────────┴────────────────┐
            │                                │
            │    SRIJ IPsec VPN Tunnel       │
            │    (Phase 1 + Phase 2)         │
            │                                │
┌───────────▼────────────┐         ┌─────────▼──────────┐
│                        │         │                    │
│  SRIJ Infrastructure   │◄────────┤  Your VPN/FW       │
│  (Turismo Portugal)    │  IPsec  │  Appliance         │
│                        │  Tunnel │  (You Control)     │
└────────────────────────┘         └─────────┬──────────┘
                                             │
                                             │ Management
                                             │ Access
                                             │
                    ┌────────────────────────┴─────────────────┐
                    │                                          │
                    │     Portuguese Data Center               │
                    │                                          │
                    │   ┌──────────────────────────────┐      │
                    │   │  DMZ Network (10.0.1.0/24)   │      │
                    │   │  ┌────────────────────────┐  │      │
                    │   │  │  Gateway Servers       │  │      │
                    │   │  │  (Player Traffic)      │  │      │
                    │   │  └────────────────────────┘  │      │
                    │   └──────────────────────────────┘      │
                    │                                          │
                    │   ┌──────────────────────────────┐      │
                    │   │  Storage Network             │      │
                    │   │  (10.0.3.0/24)              │      │
                    │   │  ┌────────────────────────┐  │      │
                    │   │  │  Safe Infrastructure   │  │      │
                    │   │  │  - FTPS Server         │  │      │
                    │   │  │  - Data Storage        │  │      │
                    │   │  └────────────────────────┘  │      │
                    │   └──────────────────────────────┘      │
                    │                                          │
                    │   ┌──────────────────────────────┐      │
                    │   │  SRIJ VPN Subnet             │      │
                    │   │  (10.0.10.0/24)             │      │
                    │   │  - Dedicated for SRIJ       │      │
                    │   │  - Routes to Safe only      │      │
                    │   └──────────────────────────────┘      │
                    │                                          │
                    └──────────────────────────────────────────┘

    Players                                     Gaming Platform
    (via .pt)                                   (External)
       │                                             │
       │                                             │
       └─────────────────►Gateway◄──────────────────┘
```

---

## Solution: Dedicated VPN/Firewall Appliance

### Option 1: Physical VPN Appliance (Recommended for Production)

Deploy a **hardware VPN/firewall** that you rack in your data center space and fully control.

#### Recommended Appliances

**1. pfSense Appliance** (Best Value)
- **Vendor**: Netgate
- **Model**: Netgate 6100 or 4200 (for your use case)
- **Specs**:
  - Intel CPU (crypto acceleration)
  - 8GB+ RAM
  - 2-4 network interfaces
  - IPsec VPN support
  - ~5-10 Gbps throughput
- **Cost**: €1,500 - €3,500
- **Why**: Open source (pfSense), full control, excellent IPsec support, web GUI
- **Control**: 100% - you configure via web interface or SSH
- **Management**: Direct access, no hosting provider needed

**2. Fortinet FortiGate** (Enterprise Option)
- **Model**: FortiGate 60F or 80F
- **Specs**:
  - Purpose-built VPN/firewall
  - Hardware crypto acceleration
  - 10+ Gbps throughput
  - Full IPsec support
- **Cost**: €1,500 - €4,000 (hardware) + annual license (€500-€1,000)
- **Why**: Enterprise-grade, excellent support, proven reliability
- **Control**: 100% - web GUI and CLI
- **Management**: FortiManager (optional) for centralized management

**3. Cisco ASA or Cisco Meraki MX** (Enterprise, Higher Cost)
- **Model**: ASA 5506-X or Meraki MX68
- **Cost**: €2,000 - €5,000 + licenses
- **Why**: Industry standard, strong IPsec support
- **Control**: 100% via ASDM (ASA) or cloud dashboard (Meraki)

**4. Sophos XG Firewall** (Good Alternative)
- **Model**: XG 106 or XG 136
- **Cost**: €1,500 - €3,000 + subscription
- **Why**: Good balance of features and cost
- **Control**: 100% via web console

#### **Recommendation**: **pfSense Netgate 6100**
- Best value for money
- Excellent IPsec support
- Full control without vendor lock-in
- Web GUI for easy configuration
- Active community and documentation
- No ongoing licensing costs (can buy support if needed)

---

### Option 2: Virtual VPN Appliance (Good for Smaller Scale)

Run a **virtual appliance** on one of your existing servers.

**Options**:
1. **pfSense VM** (Free, highly recommended)
   - Deploy as VM on VMware/KVM/Proxmox
   - Dedicate virtual NICs
   - 2 vCPU, 4GB RAM minimum
   - Cost: Free (pfSense CE)

2. **OPNsense VM** (pfSense fork, also excellent)
   - Similar to pfSense
   - Free and open source
   - Modern UI

3. **StrongSwan** (Software-only option)
   - Linux-based IPsec VPN
   - Install on Ubuntu/RHEL
   - Full control via config files
   - Steeper learning curve

**When to Use Virtual**:
- Smaller scale deployment
- Limited budget
- Testing/staging environments
- Have virtualization infrastructure

**When to Use Physical**:
- Production environment (recommended)
- Need guaranteed throughput
- Want physical separation of security functions
- Have rack space available

---

## Network Architecture with VPN Appliance

### Physical Connectivity

```
Internet Connection (from Data Center)
        │
        │ Public IP: X.X.X.X (assigned by DC)
        │
        ▼
┌───────────────────────────────────────┐
│   VPN/Firewall Appliance              │
│   (pfSense/FortiGate/etc.)            │
│                                       │
│   WAN Interface: X.X.X.X (public)     │
│   LAN Interface: 10.0.0.1             │
│   SRIJ VPN Interface: 10.0.10.1       │
│                                       │
│   ┌─────────────────────────────┐    │
│   │  IPsec VPN Tunnel to SRIJ   │    │
│   │  - Phase 1: IKEv2/IKEv1     │    │
│   │  - Phase 2: ESP             │    │
│   │  - Encryption: AES-256-GCM  │    │
│   │  - DH Group: 14/19/20       │    │
│   └─────────────────────────────┘    │
└───────┬───────────────────────────────┘
        │
        │ Layer 2 Switch
        │
        ├──────────► DMZ Network (10.0.1.0/24)
        │            - Gateway Servers
        │            - Load Balancer
        │
        ├──────────► App Network (10.0.2.0/24)
        │            - Captor Servers
        │
        ├──────────► Storage Network (10.0.3.0/24)
        │            - Safe Servers
        │            - FTPS Service
        │
        ├──────────► SRIJ VPN Subnet (10.0.10.0/24)
        │            - Routes from VPN tunnel
        │            - Access to Safe only
        │
        └──────────► Management Network (10.0.255.0/24)
                     - Admin access
                     - Monitoring
```

---

## IPsec VPN Configuration

### What SRIJ Typically Provides

When SRIJ provides VPN parameters, you'll receive something like:

**Phase 1 (IKE) Parameters**:
```
Version: IKEv2 (or IKEv1)
Authentication: Pre-shared key (PSK)
Encryption: AES-256-CBC or AES-256-GCM
Hash: SHA256 or SHA384
DH Group: 14 (2048-bit) or 19/20 (256/384-bit ECC)
Lifetime: 28800 seconds (8 hours) typical
NAT Traversal: Enabled
Dead Peer Detection: Enabled

SRIJ Public IP: [provided by SRIJ]
Your Public IP: [your data center public IP]
Pre-shared Key: [secret provided by SRIJ]
```

**Phase 2 (IPsec/ESP) Parameters**:
```
Protocol: ESP
Encryption: AES-256-CBC or AES-256-GCM
Hash: SHA256 or SHA384
PFS (Perfect Forward Secrecy): DH Group 14/19/20
Lifetime: 3600 seconds (1 hour) typical

Local Subnet: 10.0.3.0/24 (your Safe network)
Remote Subnet: [SRIJ internal network] or 0.0.0.0/0
```

---

## pfSense Configuration Example

### Step-by-Step: Configure SRIJ IPsec Tunnel in pfSense

#### 1. Access pfSense Web Interface

```
URL: https://10.0.0.1 (or whatever you set as LAN IP)
Default credentials: admin / pfsense (change immediately)
```

#### 2. Configure WAN Interface

Navigate to: **Interfaces > WAN**

```
Configuration Type: Static IPv4
IPv4 Address: [Your public IP from data center] / subnet mask
IPv4 Gateway: [Data center gateway IP]
Block private networks: Unchecked (if SRIJ uses private IPs over VPN)
Block bogon networks: Checked
```

#### 3. Create IPsec VPN Tunnel

Navigate to: **VPN > IPsec**

Click **Add P1** (Phase 1)

**Phase 1 Configuration**:
```
Disabled: Unchecked
Key Exchange version: IKEv2 (or as specified by SRIJ)
Internet Protocol: IPv4
Interface: WAN
Remote Gateway: [SRIJ public IP address]

Description: SRIJ VPN Tunnel

--- Authentication ---
Authentication Method: Mutual PSK
Negotiation mode: Main (for IKEv1) or Auto (for IKEv2)
My identifier: My IP address
Peer identifier: Peer IP address
Pre-Shared Key: [paste SRIJ-provided PSK]

--- Phase 1 Proposal (Encryption Algorithm) ---
Encryption Algorithm:
  ☑ AES 256 bits (or as specified by SRIJ)
Hash Algorithm:
  ☑ SHA256 (or as specified by SRIJ)
DH Group:
  ☑ 14 (2048 bit) (or as specified by SRIJ)

Lifetime: 28800 seconds (or as specified)

--- Advanced Options ---
Disable Rekey: Unchecked
Responder Only: Unchecked
NAT Traversal: Auto (or Force if behind NAT)
Dead Peer Detection: Enabled
  Delay: 10 seconds
  Max failures: 5
```

Click **Save**

#### 4. Create Phase 2 Entry

Click **Show Phase 2 Entries** under your Phase 1, then **Add P2**

**Phase 2 Configuration**:
```
Disabled: Unchecked
Mode: Tunnel IPv4
Description: SRIJ Safe Access

--- Local Network ---
Local Network: Network
  Address: 10.0.3.0 / 24 (your Safe network)

--- Remote Network ---
Remote Network: Network
  Address: [SRIJ internal network - provided by SRIJ]
  Or: 0.0.0.0 / 0 (if SRIJ specifies "any")

--- Phase 2 Proposal (SA/Key Exchange) ---
Protocol: ESP
Encryption Algorithms:
  ☑ AES 256 bits (or as specified)
Hash Algorithms:
  ☑ SHA256 (or as specified)
PFS key group:
  14 (2048 bit) (or as specified)

Lifetime: 3600 seconds (or as specified)
```

Click **Save**

#### 5. Apply Changes

Click **Apply Changes** at the top of the IPsec page.

#### 6. Configure Firewall Rules for VPN

Navigate to: **Firewall > Rules > IPsec**

Click **Add** to create a rule:

**Allow SRIJ Access to Safe**:
```
Action: Pass
Disabled: Unchecked
Interface: IPsec
Address Family: IPv4
Protocol: TCP

Source:
  Type: Network
  Address: [SRIJ remote network from Phase 2]

Destination:
  Type: Single host or alias
  Address: 10.0.3.10 (Safe FTPS server)

Destination Port Range:
  From: FTPS (990)
  To: FTPS (990)

Description: Allow SRIJ FTPS access to Safe
```

Add another rule for port 21 (FTP control):
```
(Same as above but Destination Port: 21)
```

Click **Save** and **Apply Changes**

#### 7. Configure NAT (if needed)

If your Safe servers are on a different subnet and need translation:

Navigate to: **Firewall > NAT > Outbound**

Usually automatic NAT is fine, but you can create manual rules if needed.

#### 8. Verify VPN Connection

Navigate to: **Status > IPsec**

You should see:
- **Status**: Established (green)
- **Phase 1**: Up
- **Phase 2**: Up

Navigate to: **Diagnostics > States** to see active connections.

---

## Firewall Rules for SRIJ VPN

### Complete Firewall Ruleset

**IPsec Interface Rules** (Firewall > Rules > IPsec):

```
# Rule 1: Allow SRIJ FTPS to Safe (port 990)
Pass | IPsec | TCP | Source: SRIJ_Network | Dest: Safe_FTPS_Server:990

# Rule 2: Allow SRIJ FTP Control to Safe (port 21)
Pass | IPsec | TCP | Source: SRIJ_Network | Dest: Safe_FTPS_Server:21

# Rule 3: Block all other SRIJ traffic
Block | IPsec | Any | Source: Any | Dest: Any | Description: Default deny
```

**WAN Interface Rules** (Firewall > Rules > WAN):

```
# Rule 1: Allow IPsec (IKE)
Pass | WAN | UDP | Source: SRIJ_Public_IP:500 | Dest: WAN_address:500

# Rule 2: Allow IPsec NAT-T (if NAT traversal enabled)
Pass | WAN | UDP | Source: SRIJ_Public_IP:4500 | Dest: WAN_address:4500

# Rule 3: Allow ESP (if not using NAT-T)
Pass | WAN | ESP | Source: SRIJ_Public_IP | Dest: WAN_address

# Rule 4: Block everything else (default deny)
Block | WAN | Any | Source: Any | Dest: Any
```

**LAN/Internal Rules**: Configure as needed for your internal traffic.

---

## Configuration Management

### Backup and Version Control

**pfSense Configuration Backup**:

1. **Automatic Backup**:
   - Navigate to: **Diagnostics > Backup & Restore**
   - Click **Download configuration as XML**
   - Store securely in version control (encrypt first)

2. **Automated Backups**:
   - Install **AutoConfigBackup** package
   - Configure to backup on every change
   - Or use **cron** to export config nightly:
   ```bash
   # SSH to pfSense
   # Copy config file
   scp /cf/conf/config.xml backup-server:/backups/pfsense-$(date +%Y%m%d).xml
   ```

3. **Version Control** (Git):
   ```bash
   # On your local machine
   mkdir pfsense-configs
   cd pfsense-configs
   git init

   # Download config
   scp admin@firewall:/cf/conf/config.xml ./config-$(date +%Y%m%d).xml

   # Commit
   git add .
   git commit -m "SRIJ VPN configuration update - Phase 2 lifetime changed"
   git push
   ```

### Configuration Documentation

Create a document tracking all SRIJ VPN settings:

```yaml
# srij-vpn-config.yaml

srij_vpn:
  provider: SRIJ - Turismo de Portugal
  connection_type: IPsec Site-to-Site

  phase1:
    version: IKEv2
    authentication: PSK
    encryption: AES-256-GCM
    hash: SHA256
    dh_group: 14
    lifetime: 28800
    nat_traversal: enabled
    dpd: enabled

  phase2:
    protocol: ESP
    encryption: AES-256-GCM
    hash: SHA256
    pfs_group: 14
    lifetime: 3600

  endpoints:
    srij_public_ip: X.X.X.X
    your_public_ip: Y.Y.Y.Y
    srij_internal_network: 10.20.30.0/24
    your_internal_network: 10.0.3.0/24

  access:
    allowed_services:
      - FTPS (port 990)
      - FTP Control (port 21)
    allowed_hosts:
      - 10.0.3.10 (Safe FTPS Server)

  contacts:
    srij_vpn_support: vpn@srij.turismodeportugal.pt
    your_vpn_admin: your-admin@company.com

  last_updated: 2025-11-10
  change_history:
    - date: 2025-11-10
      change: Initial configuration
      by: Admin
```

---

## Hardware Setup in Data Center

### Physical Installation

**What You Need to Bring/Ship to Data Center**:

1. **VPN/Firewall Appliance** (e.g., Netgate 6100)
2. **Network Cables**:
   - 1× WAN cable (to data center switch/router)
   - 1× LAN cable (to your internal switch)
   - 1× Management cable (for out-of-band access)
3. **Power Cable** (usually included with appliance)
4. **Console Cable** (for initial setup via serial)

**Installation Steps**:

1. **Rack the Appliance**:
   - Mount in your rack space (1U typically)
   - Connect to PDU for power

2. **Initial Console Setup**:
   - Connect laptop via serial console
   - Configure basic networking:
     - WAN IP (public IP from data center)
     - LAN IP (10.0.0.1 for management)
   - Set admin password

3. **Connect WAN**:
   - Cable from data center switch to WAN port
   - Verify connectivity (ping 8.8.8.8)

4. **Connect LAN**:
   - Cable from firewall LAN port to your internal switch
   - This connects to all your VLANs/subnets

5. **Verify Remote Access**:
   - Access web GUI via LAN IP: https://10.0.0.1
   - Or via public IP (if you allow admin from specific IPs)
   - Configure VPN for remote admin (OpenVPN/WireGuard)

6. **Configure SRIJ VPN** as detailed above

---

## Remote Management Setup

### Secure Remote Access to Your VPN/Firewall

You don't want to VPN to the data center provider's VPN just to access your firewall. Instead, set up your own admin VPN.

**Option 1: OpenVPN on pfSense** (Recommended)

Navigate to: **VPN > OpenVPN > Wizards**

1. **Create Certificate Authority** (if not already done)
2. **Create Server Certificate**
3. **Configure OpenVPN Server**:
   ```
   Server mode: Remote Access (SSL/TLS)
   Protocol: UDP on IPv4 only
   Interface: WAN
   Local port: 1194 (or custom port)
   Tunnel Network: 10.0.100.0/24 (admin VPN subnet)

   Client Settings:
     - Redirect Gateway: Unchecked (split tunnel)
     - DNS: Your internal DNS or 10.0.0.1
   ```

4. **Create User Certificates** for each admin
5. **Export OpenVPN Client Config** and distribute to admins
6. **Connect from anywhere**:
   ```bash
   openvpn --config admin-vpn.ovpn
   ```

   Then access pfSense: https://10.0.0.1

**Option 2: WireGuard on pfSense**

Install **WireGuard package**:
- Navigate to: **System > Package Manager**
- Install **wireguard**
- Configure similarly to OpenVPN
- Faster, modern, simpler

**Firewall Rule for Admin VPN Access**:
```
Pass | OpenVPN | Any | Source: OpenVPN_Clients | Dest: Firewall:443 | Description: Admin access to firewall
```

---

## Monitoring and Troubleshooting

### Monitor VPN Status

**pfSense Dashboard**:

Navigate to: **Status > Dashboard**

Add widgets:
- **IPsec**: Shows tunnel status
- **Gateway**: Shows WAN connectivity
- **System Information**: CPU/memory usage

**IPsec Status**:

Navigate to: **Status > IPsec**

Check:
- ✅ **Phase 1**: Established
- ✅ **Phase 2**: Established
- ⚠️ **Warning**: If status is "connecting", check Phase 1/2 settings
- ❌ **Error**: If status is "down", check WAN connectivity and SRIJ side

**View Logs**:

Navigate to: **Status > System Logs > IPsec**

Look for:
- IKE negotiations
- Phase 1 establishment
- Phase 2 establishment
- Errors (mismatch in crypto settings, PSK wrong, etc.)

### Common Issues and Solutions

**Issue 1: VPN Tunnel Won't Establish**

Symptoms:
- Phase 1 shows "connecting" or down
- Logs show "no suitable proposal found"

Solutions:
- Verify Phase 1 encryption settings match SRIJ exactly
- Check DH group matches
- Verify PSK is correct (no extra spaces)
- Confirm SRIJ's public IP is correct in config
- Verify your public IP hasn't changed

**Issue 2: Phase 1 Up, Phase 2 Won't Establish**

Symptoms:
- Phase 1 shows established
- Phase 2 shows "connecting" or down

Solutions:
- Verify Phase 2 encryption settings match SRIJ
- Check PFS group matches
- Verify local subnet (10.0.3.0/24) is correct
- Verify remote subnet matches what SRIJ expects
- Check if proxy-id/traffic selectors match

**Issue 3: VPN Established but No Traffic**

Symptoms:
- Both Phase 1 and Phase 2 show established
- Cannot FTPS to Safe from SRIJ

Solutions:
- Check firewall rules on IPsec interface (allow SRIJ → Safe)
- Verify routing: Does Safe server know to route back via firewall?
- Check if Safe FTPS service is actually running: `netstat -tlnp | grep 990`
- Test connectivity from firewall: `tcpdump -i ipsec1000 port 990`

**Issue 4: NAT Traversal Issues**

Symptoms:
- Phase 1 keeps dropping
- Logs mention NAT detection

Solutions:
- Enable NAT Traversal in Phase 1 settings
- Verify UDP port 4500 is allowed on WAN firewall rules
- Check if data center has NAT between you and internet

### Packet Capture for Troubleshooting

**Capture FTPS Traffic from SRIJ**:

Navigate to: **Diagnostics > Packet Capture**

```
Interface: IPsec (ipsec1000)
Address Family: IPv4
Protocol: TCP
Host: 10.0.3.10 (Safe server)
Port: 990
Packet Count: 100

Click Start
```

This shows you exactly what traffic is (or isn't) coming over the VPN tunnel.

---

## Cost Breakdown for VPN Solution

### Hardware Option

**Physical Appliance** (Netgate pfSense 6100):
```
Equipment:
- Netgate 6100: €2,500
- Rack rails: €100
- Cables: €50
- Shipping to Portugal: €100

One-time total: €2,750

Recurring:
- Data center rack space: Included (already have racks)
- Power: €5-10/month (minimal)
- TAC Support (optional): €500/year

Annual cost: €60-€120 (power) + optional support
```

**Virtual Appliance** (pfSense VM):
```
Equipment:
- None (use existing server)
- pfSense CE: Free

One-time total: €0

Recurring:
- Resource allocation on existing server: €0

Annual cost: €0
```

### Total Cost Comparison

| Solution | One-Time | Annual | Control | Best For |
|----------|----------|--------|---------|----------|
| Physical pfSense | €2,750 | €60-€620 | 100% | Production |
| Virtual pfSense | €0 | €0 | 100% | Small/staging |
| Physical FortiGate | €3,500 | €1,000 | 100% | Enterprise |
| Hosting provider VPN | €0 | €500-€2,000 | 20% | Not recommended |

**Recommendation**: Physical pfSense for production, virtual for staging/development.

---

## Integration with Existing Architecture

### Updated Network Diagram

```
                    Internet
                       │
                       │ Public IP
                       │
                       ▼
    ┌──────────────────────────────────────┐
    │                                      │
    │  Your VPN/Firewall Appliance         │
    │  (pfSense / FortiGate)               │
    │                                      │
    │  ┌────────────────────────────┐     │
    │  │ IPsec to SRIJ              │     │
    │  │ - Phase 1: AES-256/SHA256  │     │
    │  │ - Phase 2: ESP/AES-256     │     │
    │  │ - Routes: SRIJ ↔ Safe      │     │
    │  └────────────────────────────┘     │
    │                                      │
    │  ┌────────────────────────────┐     │
    │  │ OpenVPN Server             │     │
    │  │ - Admin remote access      │     │
    │  └────────────────────────────┘     │
    │                                      │
    └──────┬────────────────────┬──────────┘
           │                    │
           │ All Internal       │ SRIJ VPN
           │ Networks           │ Traffic Only
           │                    │
           ▼                    ▼
    ┌─────────────┐      ┌──────────────┐
    │  DMZ        │      │  SRIJ VPN    │
    │  10.0.1.0   │      │  Subnet      │
    │             │      │  10.0.10.0   │
    │  Gateway ───┤      │              │
    └─────────────┘      │  Routes to:  │
                         │  - Safe      │
    ┌─────────────┐      │    10.0.3.10 │
    │  App Net    │      └──────────────┘
    │  10.0.2.0   │              │
    │             │              │
    │  Captor ────┤              │
    └─────────────┘              │
                                 │
    ┌─────────────┐              │
    │  Storage    │◄─────────────┘
    │  10.0.3.0   │
    │             │
    │  Safe ──────┤ FTPS: 990, 21
    │  10.0.3.10  │
    └─────────────┘

    ┌─────────────┐
    │  Mgmt Net   │
    │  10.0.255.0 │
    │             │
    │  Monitor ───┤
    │  Jump Host ─┤
    └─────────────┘
```

### Static Routes

**On pfSense/Firewall**:
- Default route → Data center gateway (for internet)
- SRIJ remote network → IPsec tunnel (automatic when tunnel is up)

**On Safe Server** (10.0.3.10):
```bash
# Ensure default gateway points to firewall
ip route add default via 10.0.3.1 (firewall IP on storage network)
```

**On Other Servers**:
- Default route → Firewall (10.0.x.1 on their respective subnets)

---

## Deployment Checklist

### VPN Appliance Deployment

**Phase 1: Procurement**
- [ ] Select VPN appliance (recommended: Netgate pfSense 6100)
- [ ] Order equipment
- [ ] Order any additional cables/accessories
- [ ] Receive and test locally (pre-configure if possible)

**Phase 2: Pre-Configuration** (Do this locally before shipping to DC)
- [ ] Unbox and power on appliance
- [ ] Connect via serial console
- [ ] Set basic config:
  - [ ] Hostname: `pfsense-gateway`
  - [ ] Domain: `yourdomain.pt`
  - [ ] LAN IP: 10.0.0.1/24
  - [ ] Admin password: [strong password]
  - [ ] Time zone: Europe/Lisbon
- [ ] Access web GUI
- [ ] Update to latest pfSense version
- [ ] Configure admin VPN (OpenVPN) for remote access
- [ ] Document configuration
- [ ] Backup configuration

**Phase 3: Installation at Data Center**
- [ ] Ship appliance to Portuguese data center
- [ ] Coordinate with data center for installation date
- [ ] Data center racks appliance
- [ ] Data center connects WAN (public IP from DC)
- [ ] Data center connects LAN (to your internal switch)
- [ ] Data center powers on appliance

**Phase 4: WAN Configuration** (Remote via OpenVPN or console)
- [ ] Connect via console or temporary management access
- [ ] Configure WAN interface with public IP
- [ ] Configure WAN gateway
- [ ] Test internet connectivity (ping 8.8.8.8)
- [ ] Configure firewall rules for admin access
- [ ] Connect via OpenVPN from your office
- [ ] Verify remote management works

**Phase 5: SRIJ VPN Configuration**
- [ ] Receive Phase 1 and Phase 2 parameters from SRIJ
- [ ] Configure IPsec Phase 1 in pfSense
- [ ] Configure IPsec Phase 2 in pfSense
- [ ] Configure firewall rules (IPsec interface)
- [ ] Save and apply configuration
- [ ] Contact SRIJ to initiate tunnel from their side
- [ ] Verify tunnel establishment (Status > IPsec)
- [ ] Test connectivity: SRIJ → Safe FTPS

**Phase 6: Internal Network Configuration**
- [ ] Connect internal switch to firewall LAN
- [ ] Configure VLANs (DMZ, App, Storage, SRIJ VPN, Mgmt)
- [ ] Configure DHCP (if needed) or static IPs
- [ ] Configure DNS (pfSense can be DNS server)
- [ ] Configure NAT rules (if needed)
- [ ] Test internal connectivity

**Phase 7: Testing and Validation**
- [ ] Test SRIJ VPN tunnel is stable (24+ hours)
- [ ] Test SRIJ can FTPS to Safe (port 990)
- [ ] Test Safe can respond to SRIJ
- [ ] Test failover if redundant setup
- [ ] Load test (if applicable)
- [ ] Document all IP addresses and configs

**Phase 8: Monitoring Setup**
- [ ] Configure syslog to central logging server
- [ ] Set up alerts (VPN down, high CPU, etc.)
- [ ] Add to monitoring system (Prometheus/Grafana)
- [ ] Create dashboard for VPN status
- [ ] Set up automated config backups

**Phase 9: Documentation**
- [ ] Document complete configuration
- [ ] Create network diagram
- [ ] Write troubleshooting guide
- [ ] Document SRIJ contacts
- [ ] Create runbook for common tasks
- [ ] Store configuration backups securely

**Phase 10: Handover**
- [ ] Train operations team
- [ ] Provide admin credentials (secure storage)
- [ ] Provide OpenVPN configs for admins
- [ ] Schedule regular maintenance windows
- [ ] Establish change control process

---

## Ongoing Management

### Regular Tasks

**Daily**:
- [ ] Check VPN status dashboard
- [ ] Review alerts for VPN issues

**Weekly**:
- [ ] Review IPsec logs for anomalies
- [ ] Check for firmware updates
- [ ] Verify backup exists and is recent
- [ ] Review firewall logs for unauthorized access

**Monthly**:
- [ ] Update pfSense to latest stable version (if available)
- [ ] Review and optimize firewall rules
- [ ] Test SRIJ VPN failover (if redundant)
- [ ] Review performance metrics
- [ ] Update documentation if changes made

**Quarterly**:
- [ ] Rotate admin VPN certificates
- [ ] Security audit of firewall rules
- [ ] Disaster recovery test (restore from backup)
- [ ] Review and renew SRIJ VPN settings (if needed)

**Annually**:
- [ ] Hardware health check
- [ ] Consider hardware refresh (5-year cycle)
- [ ] Review and update support contracts
- [ ] Full security audit

### Change Management

**When SRIJ Changes VPN Parameters**:

1. **Notification**: SRIJ emails new Phase 1/2 parameters
2. **Schedule Maintenance Window**: Coordinate with SRIJ
3. **Backup Current Config**: Download XML backup
4. **Make Changes**: Update Phase 1/2 in pfSense
5. **Test**: Verify tunnel re-establishes
6. **Document**: Update documentation and commit to git
7. **Notify Team**: Email operations team of changes

---

## Summary

### Key Advantages of This Architecture

✅ **Full Control**: You manage VPN configuration, no hosting provider involvement
✅ **Flexibility**: Change firewall rules, VPN settings anytime
✅ **Cost-Effective**: One-time hardware cost (~€2,750), minimal recurring
✅ **Security**: You control the security perimeter
✅ **Transparency**: Full visibility into VPN status and logs
✅ **Independence**: Not locked into hosting provider's equipment
✅ **Scalability**: Easy to upgrade or add redundancy
✅ **Remote Management**: Manage from anywhere via admin VPN

### What You Control

- ✅ IPsec VPN configuration (Phase 1, Phase 2)
- ✅ Firewall rules
- ✅ Network segmentation (VLANs)
- ✅ NAT rules
- ✅ Routing
- ✅ Monitoring and logging
- ✅ Admin access
- ✅ Backup and recovery
- ✅ Updates and patching

### What You Don't Need from Hosting Provider

- ❌ VPN configuration (you do it yourself)
- ❌ Firewall changes (you do it yourself)
- ❌ Network changes (you do it yourself on your equipment)
- ❌ Constant support tickets for every change

### Recommended Solution

**For Production**: **Netgate pfSense 6100** (~€2,500)
- Physical appliance
- Full control
- Excellent IPsec support
- Web GUI (easy configuration)
- Remote management via OpenVPN
- No ongoing licensing

**For Staging/Development**: **pfSense VM** (Free)
- Virtual appliance on existing server
- Test VPN configurations before production
- Full feature parity with physical

---

## Next Steps

1. **Order pfSense Appliance**: Netgate 6100 from netgate.com
2. **Pre-configure Locally**: Set up basic networking and admin VPN
3. **Ship to Data Center**: Coordinate installation
4. **Contact SRIJ**: Request VPN parameters (Phase 1/2)
5. **Configure VPN**: Follow configuration guide above
6. **Test**: Verify SRIJ can access Safe via VPN
7. **Document**: Save config and document all settings

---

**Document Version**: 1.0
**Last Updated**: 2025-11-10
**Author**: Technical Architecture Team

**Related Documents**:
- TECHNICAL_ARCHITECTURE.md (main architecture)
- SRIJ_ERI_REQUIREMENTS.md (requirements)
- HOSTING_REQUIREMENTS.md (data center selection)
