# pfSense Quick Reference - SRIJ VPN Configuration

## Quick Start Guide

This is a condensed reference for configuring your pfSense appliance for SRIJ VPN connectivity. For complete details, see VPN_TUNNEL_ARCHITECTURE.md.

---

## Hardware Recommendation

**Netgate 6100** (~€2,500)
- 4-core Intel CPU with AES-NI
- 8GB RAM
- 128GB SSD
- 4× 1GbE ports
- Purchase: https://www.netgate.com/

---

## Initial Setup (Local, Before Shipping to Data Center)

### 1. Console Access

Connect via serial console:
```
Speed: 115200 baud
Data bits: 8
Stop bits: 1
Parity: None
Flow control: None
```

### 2. Basic Configuration

At console menu:
```
1) Assign Interfaces
   WAN: em0 (or appropriate interface)
   LAN: em1

2) Set Interface IP Address
   LAN: 10.0.0.1/24

8) Shell
   Set timezone: Select 26 (Europe/Lisbon)
   Exit: exit
```

### 3. Web GUI Access

From laptop on LAN:
```
URL: https://10.0.0.1
Username: admin
Password: pfsense (change immediately!)
```

### 4. Setup Wizard

Navigate to: **System > Setup Wizard**

```
General Information:
  Hostname: pfsense-gateway
  Domain: yourdomain.pt
  Primary DNS: 8.8.8.8
  Secondary DNS: 8.8.4.4
  Override DNS: Checked

Time Server:
  Timezone: Europe/Lisbon
  Timeserver: ntp.oal.ul.pt (Lisbon Observatory)

WAN Configuration: (leave for later, will set at data center)

LAN Configuration:
  IP: 10.0.0.1
  Subnet: 24

Admin Password:
  Set strong password
```

### 5. Update Firmware

Navigate to: **System > Update**

Click **Confirm** to update to latest stable version.

### 6. Enable SSH (Optional, for remote management)

Navigate to: **System > Advanced > Admin Access**

```
Secure Shell:
  ☑ Enable Secure Shell
  SSH port: 22 (or custom)
  ☑ Disable password login for Secure Shell (after setting up keys)
```

---

## Admin VPN Setup (OpenVPN for Remote Management)

### Step 1: Certificate Authority

Navigate to: **System > Cert Manager > CAs**

Click **Add**:
```
Descriptive name: Internal CA
Method: Create an internal Certificate Authority
Key length: 2048 bit
Digest Algorithm: SHA256
Lifetime: 3650 days (10 years)
Common Name: internal-ca
```

### Step 2: Server Certificate

Navigate to: **System > Cert Manager > Certificates**

Click **Add/Sign**:
```
Method: Create an Internal Certificate
Descriptive name: OpenVPN Server
Certificate Authority: Internal CA
Key length: 2048 bit
Digest Algorithm: SHA256
Certificate Type: Server Certificate
Lifetime: 3650 days
Common Name: openvpn-server
```

### Step 3: OpenVPN Server

Navigate to: **VPN > OpenVPN > Wizards**

Select: **Local User Access**

**Type of Server**: Local User Access

**Certificate Authority**: Internal CA

**Server Certificate**: OpenVPN Server (create new if needed)

**General OpenVPN Server Information**:
```
Interface: WAN
Protocol: UDP on IPv4 only
Local Port: 1194
Description: Admin VPN
```

**Cryptographic Settings**:
```
TLS Authentication: ☑ Enabled
DH Parameter Length: 2048 bit
Encryption Algorithm: AES-256-CBC
Auth Digest Algorithm: SHA256
Hardware Crypto: Intel RDRAND engine - RAND
```

**Tunnel Settings**:
```
Tunnel Network: 10.0.100.0/24
Local Network: 10.0.0.0/16 (or all internal subnets)
Concurrent Connections: 10
Inter-Client Communication: Unchecked
Duplicate Connection: Unchecked
```

**Client Settings**:
```
Dynamic IP: Checked
DNS Default Domain: yourdomain.pt
DNS Server 1: 10.0.0.1 (pfSense)
```

Click **Next** through remaining screens.

### Step 4: Create User

Navigate to: **System > User Manager > Users**

Click **Add**:
```
Username: admin-user
Password: [strong password]
Full name: Admin User

Certificate: ☑ Click to create a user certificate
  Method: Create an internal certificate
  Descriptive name: admin-user-cert
```

Click **Save**

### Step 5: Export Client Config

Navigate to: **VPN > OpenVPN > Client Export**

```
Remote Access Server: Admin VPN
Host Name Resolution: Other (specify below)
  Host Name: [your-public-ip-or-hostname]

Export Type: Archive
  Select user: admin-user
  Click download config
```

Install OpenVPN on your laptop, import config, and connect!

---

## SRIJ IPsec VPN Configuration

### Phase 1 (IKE)

Navigate to: **VPN > IPsec > Tunnels**

Click **Add P1**

```
General Information:
  ☐ Disabled
  Key Exchange version: IKEv2
  Internet Protocol: IPv4
  Interface: WAN
  Remote Gateway: [SRIJ_PUBLIC_IP]
  Description: SRIJ VPN Tunnel

Phase 1 Proposal (Authentication):
  Authentication Method: Mutual PSK
  Negotiation mode: Main (for IKEv1) / Auto (for IKEv2)
  My identifier: My IP address
  Peer identifier: Peer IP address
  Pre-Shared Key: [PASTE_SRIJ_PSK_HERE]

Phase 1 Proposal (Encryption Algorithm):
  Encryption Algorithm:
    ☑ AES 256 bits  [Select as specified by SRIJ]
  Hash Algorithm:
    ☑ SHA256  [Select as specified by SRIJ]
  DH Group:
    ☑ 14 (2048 bit)  [Select as specified by SRIJ]
  Lifetime: 28800  [As specified by SRIJ]

Advanced Options:
  ☐ Disable Rekey
  ☐ Responder Only
  ☐ NAT Traversal: Auto
  ☑ Enable DPD
    Delay: 10
    Max failures: 5
```

Click **Save**

### Phase 2 (ESP)

Under your Phase 1 entry, click **Show Phase 2 Entries**

Click **Add P2**

```
General Information:
  ☐ Disabled
  Mode: Tunnel IPv4
  Description: SRIJ Safe Access

Local Network:
  Local Network: Network
    Address: 10.0.3.0 / 24  [Your Safe network]

Remote Network:
  Remote Network: Network
    Address: [SRIJ_NETWORK]  [As specified by SRIJ]
    Or: 0.0.0.0 / 0  [If SRIJ specifies "any"]

Phase 2 Proposal (SA/Key Exchange):
  Protocol: ESP
  Encryption Algorithms:
    ☑ AES 256 bits  [As specified by SRIJ]
  Hash Algorithms:
    ☑ SHA256  [As specified by SRIJ]
  PFS key group: 14 (2048 bit)  [As specified by SRIJ]
  Lifetime: 3600  [As specified by SRIJ]
```

Click **Save**

Click **Apply Changes**

---

## Firewall Rules

### WAN Rules (Allow IPsec)

Navigate to: **Firewall > Rules > WAN**

Click **Add** (top, to add to beginning)

```
Action: Pass
Disabled: ☐
Interface: WAN
Address Family: IPv4
Protocol: UDP

Source:
  Type: Single host or alias
  Address: [SRIJ_PUBLIC_IP]

Destination:
  Type: WAN address

Destination Port Range:
  From: ISAKMP (500)
  To: ISAKMP (500)

Description: Allow SRIJ IKE (Phase 1)
```

Add another for NAT-T:
```
(Same as above but port 4500)
Description: Allow SRIJ IPsec NAT-T
```

If not using NAT-T, add ESP rule:
```
Protocol: ESP
Source: [SRIJ_PUBLIC_IP]
Destination: WAN address
Description: Allow SRIJ ESP
```

### IPsec Rules (Allow SRIJ to Safe)

Navigate to: **Firewall > Rules > IPsec**

Click **Add**

```
Action: Pass
Disabled: ☐
Interface: IPsec
Address Family: IPv4
Protocol: TCP

Source:
  Type: Network
  Address: [SRIJ_NETWORK]  [As in Phase 2]

Destination:
  Type: Single host or alias
  Address: 10.0.3.10  [Your Safe FTPS server]

Destination Port Range:
  From: FTPS (990)
  To: FTPS (990)

Description: Allow SRIJ FTPS to Safe (990)
```

Add another for FTP control:
```
(Same as above but port 21)
Description: Allow SRIJ FTP control to Safe (21)
```

Add a block rule at the end:
```
Action: Block
Protocol: Any
Source: Any
Destination: Any
Description: Default deny SRIJ VPN traffic
```

---

## Verification

### Check VPN Status

Navigate to: **Status > IPsec**

You should see:
```
Status: Established (green)
Phase 1: Up, AES-256-GCM, SHA256, DH Group 14
Phase 2: Up, AES-256-GCM, SHA256, PFS Group 14
```

### View Logs

Navigate to: **Status > System Logs > IPsec**

Look for:
```
✅ Successfully established IKE SA
✅ Successfully established IPsec SA
✅ Child SA established
```

### Test Connectivity

From pfSense shell:

```bash
# Ping SRIJ remote network (if allowed)
ping [SRIJ_REMOTE_IP]

# Test FTPS port to Safe (from SRIJ side, coordinate with SRIJ)
# They should be able to:
telnet 10.0.3.10 990
```

### Packet Capture

Navigate to: **Diagnostics > Packet Capture**

```
Interface: IPsec (ipsec1000)
Address Family: IPv4
Protocol: TCP
Host: 10.0.3.10
Port: 990
```

Click **Start** and watch for SRIJ traffic.

---

## Common Configurations

### Add VLAN

Navigate to: **Interfaces > Assignments > VLANs**

Click **Add**:
```
Parent Interface: em1 (LAN)
VLAN Tag: 3  [For storage network]
Description: Storage
```

Navigate to: **Interfaces > Assignments**

Add new interface, select VLAN, assign.

Navigate to: **Interfaces > STORAGE**

```
Enable: ☑ Enable interface
IPv4 Configuration Type: Static IPv4
IPv4 Address: 10.0.3.1 / 24
```

### Add Firewall Alias (for easy management)

Navigate to: **Firewall > Aliases > IP**

Click **Add**:
```
Name: SRIJ_Network
Type: Network(s)
IP or FQDN: [SRIJ_NETWORK]/[MASK]
Description: SRIJ Remote Network
```

Now you can use `SRIJ_Network` in firewall rules instead of typing IP every time.

### Set up Automatic Config Backup

Navigate to: **Diagnostics > Command Prompt**

Create a script:
```bash
#!/bin/sh
# /root/backup-config.sh

DATE=$(date +%Y%m%d-%H%M%S)
cp /cf/conf/config.xml /root/backups/config-$DATE.xml

# Keep only last 30 backups
ls -t /root/backups/config-*.xml | tail -n +31 | xargs rm -f

# Optional: SCP to remote server
# scp /cf/conf/config.xml user@backup-server:/backups/pfsense-config.xml
```

Add to cron:

Navigate to: **Services > Cron**

Click **Add**:
```
Minute: 0
Hour: 2
Day of Month: *
Month: *
Day of Week: *
User: root
Command: /root/backup-config.sh
```

---

## Troubleshooting Quick Reference

### VPN Won't Establish

**Check Phase 1**:
- Verify SRIJ public IP is correct
- Verify your public IP hasn't changed
- Check PSK (no spaces, correct copy/paste)
- Verify encryption settings match SRIJ exactly
- Check DH group matches

**Check Phase 2**:
- Verify local subnet is correct (10.0.3.0/24)
- Verify remote subnet matches SRIJ
- Check PFS group matches
- Verify encryption settings match

**Check Firewall**:
- WAN rules allow UDP 500 (IKE) from SRIJ IP
- WAN rules allow UDP 4500 (NAT-T) if NAT traversal enabled
- Or allow ESP if not using NAT-T

### VPN Established but No Traffic

**Check**:
- IPsec firewall rules allow SRIJ → Safe on port 990/21
- Safe server is reachable from pfSense: `ping 10.0.3.10`
- Safe FTPS service is running: `telnet 10.0.3.10 990`
- Routing on Safe points back to pfSense: `route -n`

**Test from pfSense**:
```bash
# Shell access
8) Shell

# Ping Safe
ping 10.0.3.10

# Test FTPS port
telnet 10.0.3.10 990
```

### NAT Issues

If behind NAT at data center:
- Enable NAT Traversal in Phase 1
- Ensure UDP 4500 is allowed on WAN
- Check NAT-T is actually being used (Status > IPsec)

### High CPU Usage

Check:
- Hardware crypto is enabled (System > Advanced > Miscellaneous)
- AES-NI is available (Status > System)
- Not using software crypto unnecessarily

---

## Configuration Backup

### Manual Backup

Navigate to: **Diagnostics > Backup & Restore**

Click **Download configuration as XML**

Store securely! This file contains all config including passwords.

### Restore Configuration

Navigate to: **Diagnostics > Backup & Restore**

Choose file, click **Restore Configuration**

---

## Useful Commands (Shell Access)

```bash
# Access shell
Option 8 from console menu

# View IPsec status
ipsec statusall

# Restart IPsec
ipsec restart

# View routing table
netstat -rn

# View active connections
sockstat -4

# Test network connectivity
ping 8.8.8.8
ping 10.0.3.10

# View live logs
clog -f /var/log/system.log
clog -f /var/log/ipsec.log

# Packet capture
tcpdump -i em0  # WAN
tcpdump -i ipsec1000 port 990  # VPN tunnel, FTPS

# Reboot
reboot

# Shutdown
halt
```

---

## Emergency Access

### Console Recovery

If locked out of web GUI:

1. Connect via serial console
2. Select option **8) Shell**
3. Reset admin password:
   ```bash
   pfSsh.php playback changepassword
   ```
4. Enter new password when prompted

### Reset to Factory Defaults

**WARNING**: This erases all configuration!

From console menu:
```
4) Reset to factory defaults
```

Or from shell:
```bash
rm /cf/conf/config.xml
reboot
```

---

## Support Resources

**pfSense Documentation**:
- Official docs: https://docs.netgate.com/pfsense/
- Forum: https://forum.netgate.com/
- Reddit: r/PFSENSE

**IPsec Troubleshooting**:
- https://docs.netgate.com/pfsense/en/latest/vpn/ipsec/ipsec-troubleshooting.html

**Book**:
- "pfSense: The Definitive Guide" by Christopher M. Buechler and Jim Pingle

---

## Quick Reference Card

### Access

| Interface | URL/Method | Default Creds |
|-----------|------------|---------------|
| Web GUI | https://10.0.0.1 | admin / pfsense |
| SSH | ssh admin@10.0.0.1 | (same) |
| Console | Serial 115200 8N1 | (same) |

### Important Directories

| Path | Description |
|------|-------------|
| /cf/conf/config.xml | Main configuration file |
| /var/log/ | Log files |
| /tmp/ | Temporary files |
| /root/ | Root home directory |

### Key Settings Locations

| Setting | Path |
|---------|------|
| IPsec VPN | VPN > IPsec |
| Firewall Rules | Firewall > Rules |
| Interfaces | Interfaces > Assignments |
| System Logs | Status > System Logs |
| Backup Config | Diagnostics > Backup & Restore |
| Package Manager | System > Package Manager |

### SRIJ-Specific Quick Checks

```bash
# Is SRIJ VPN up?
Navigate to: Status > IPsec
Look for: Established (green)

# Can SRIJ reach Safe?
Navigate to: Diagnostics > Packet Capture
Interface: IPsec
Host: 10.0.3.10
Port: 990
Click Start, watch for traffic from SRIJ

# View SRIJ access logs
Navigate to: Status > System Logs > Firewall
Filter for: 10.0.3.10
```

---

## Change Log Template

Keep a change log for your pfSense config:

```markdown
# pfSense Change Log

## 2025-11-15 - Initial SRIJ VPN Configuration
- Created Phase 1 to SRIJ (IKEv2, AES-256-GCM, SHA256, DH14)
- Created Phase 2 (ESP, AES-256-GCM, SHA256, PFS14)
- Added firewall rules (WAN: UDP 500/4500, IPsec: TCP 990/21)
- Tunnel established successfully
- SRIJ confirmed connectivity to Safe
- By: Admin User

## 2025-11-20 - Updated Phase 2 Lifetime
- SRIJ requested lifetime change from 3600 to 7200 seconds
- Updated Phase 2 settings
- Tunnel re-established without issues
- By: Admin User
```

---

**Document Version**: 1.0
**Last Updated**: 2025-11-10

**Related Documents**:
- VPN_TUNNEL_ARCHITECTURE.md (complete VPN guide)
- TECHNICAL_ARCHITECTURE.md (overall architecture)
- DEPLOYMENT_CHECKLIST.md (deployment steps)
