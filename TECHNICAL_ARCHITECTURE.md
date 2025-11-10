# SRIJ ERI Gateway - Technical Architecture

## Architecture Overview

This document provides the technical architecture for implementing a compliant SRIJ Entry and Registry Infrastructure (ERI) gateway for online gambling operations in Portugal.

---

## 1. High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         Portuguese Players                          │
│                    (Portuguese IPs / .pt accounts)                  │
└───────────────────────────────┬─────────────────────────────────────┘
                                │
                                │ HTTPS (via .pt domain)
                                ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    GATEWAY (Portuguese Territory)                   │
│  ┌──────────────────────────────────────────────────────────────┐  │
│  │                    Load Balancer / Proxy                     │  │
│  │              (IP Geolocation & Traffic Routing)              │  │
│  └──────────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────────┐  │
│  │                      Gateway Servers                          │  │
│  │           - Traffic inspection and logging                    │  │
│  │           - Player session management                         │  │
│  │           - Portuguese compliance enforcement                 │  │
│  └──────────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────────┐  │
│  │                         CAPTOR                                │  │
│  │           - Captures all gaming events                        │  │
│  │           - Real-time data collection                         │  │
│  │           - Data categorization per SRIJ model                │  │
│  └──────────────────────────────────────────────────────────────┘  │
└───────────────────────┬─────────────────────┬───────────────────────┘
                        │                     │
                        │                     │ Secure Protocol
                        │                     │ (FTPS/HTTPS)
                        │                     ▼
                        │     ┌───────────────────────────────────────┐
                        │     │    SAFE (Portuguese Territory)        │
                        │     │  ┌─────────────────────────────────┐  │
                        │     │  │    Data Processing Layer        │  │
                        │     │  │  - Signing                      │  │
                        │     │  │  - Compression                  │  │
                        │     │  │  - Encryption (X.509 v3)        │  │
                        │     │  └─────────────────────────────────┘  │
                        │     │  ┌─────────────────────────────────┐  │
                        │     │  │    Storage Layer                │  │
                        │     │  │  - 24-month hot storage         │  │
                        │     │  │  - 96-month cold storage        │  │
                        │     │  │  - SRIJ folder structure        │  │
                        │     │  └─────────────────────────────────┘  │
                        │     │  ┌─────────────────────────────────┐  │
                        │     │  │    FTPS Server                  │  │
                        │     │  │  - SRIJ permanent access        │  │
                        │     │  └─────────────────────────────────┘  │
                        │     └───────────────┬───────────────────────┘
                        │                     │
                        │                     │ FTPS (Port 990/21)
                        │                     ▼
                        │     ┌───────────────────────────────────────┐
                        │     │   SRIJ Control Infrastructure         │
                        │     │  - Data collection                    │
                        │     │  - Audit and inspection               │
                        │     │  - Compliance monitoring              │
                        │     └───────────────────────────────────────┘
                        │
                        │ HTTPS/WSS
                        ▼
┌─────────────────────────────────────────────────────────────────────┐
│              Main Gaming Platform (Can be outside Portugal)         │
│  ┌──────────────────────────────────────────────────────────────┐  │
│  │                    Gaming Servers                             │  │
│  │           - Game logic and RNG                                │  │
│  │           - Player account management                         │  │
│  │           - Payment processing                                │  │
│  └──────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                   NTP Time Synchronization                          │
│          Lisbon Astronomical Observatory NTP Servers                │
│                  (All components synchronized)                      │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 2. Component Architecture

### 2.1 Gateway Component

#### 2.1.1 Technology Stack (Recommended)
```
- Load Balancer: HAProxy / Nginx / AWS ALB
- Reverse Proxy: Nginx / Apache
- Application Layer: Node.js / Python / Java
- Geolocation: MaxMind GeoIP2 / IP2Location
- Monitoring: Prometheus + Grafana
- Logging: ELK Stack (Elasticsearch, Logstash, Kibana)
```

#### 2.1.2 Gateway Functions
1. **Traffic Routing**
   - Receive all inbound traffic on .pt domain
   - Validate player origin (IP geolocation)
   - Route to main gaming platform
   - Return responses to players

2. **IP Geolocation Detection**
   ```javascript
   // Pseudocode example
   function isPortuguesePlayer(request) {
       const clientIP = getClientIP(request);
       const geoData = geolocate(clientIP);

       if (geoData.country === 'PT') {
           return true;
       }

       // Also check if account is registered in Portugal
       const account = getPlayerAccount(request);
       if (account.registeredCountry === 'PT') {
           return true;
       }

       return false;
   }
   ```

3. **Domain Enforcement**
   - Ensure Portuguese players only access via .pt domain
   - Redirect if accessing from wrong domain

4. **Event Logging**
   - Log all player activities
   - Send events to Captor in real-time

#### 2.1.3 Gateway Server Specifications (Minimum)
```
CPU: 4+ cores
RAM: 8GB+
Storage: 100GB SSD (for logs and temporary data)
Network: 1 Gbps NIC
OS: Linux (Ubuntu 22.04 LTS / RHEL 8 / Oracle Linux)
```

---

### 2.2 Captor Component

#### 2.2.1 Technology Stack (Recommended)
```
- Message Queue: Apache Kafka / RabbitMQ / Redis Streams
- Processing: Apache Flink / Spark Streaming / Custom application
- Language: Java / Python / Go
- Database: PostgreSQL (for temporary buffering)
```

#### 2.2.2 Captor Functions
1. **Event Capture**
   - Receive events from Gateway
   - Capture all gaming-related activities:
     - Player logins/logouts
     - Bets placed
     - Game outcomes
     - Withdrawals/deposits
     - Account changes

2. **Data Categorization**
   - Categorize data according to SRIJ Data Model
   - Structure data for Safe storage
   - Apply metadata and timestamps

3. **Performance Requirements**
   - Process events at platform speed (no bottlenecks)
   - Handle peak traffic (e.g., major sporting events)
   - Buffer management for temporary network issues

4. **Data Transfer**
   - Secure transfer to Safe (FTPS/HTTPS)
   - Retry logic for failed transfers
   - Integrity verification

#### 2.2.3 Data Categories (SRIJ Model)
Based on SRIJ requirements, typical categories include:
- Player registration data
- Authentication events
- Transaction data (deposits, withdrawals)
- Betting data (stakes, odds, outcomes)
- Game session data
- Player balance changes
- Bonus and promotion data
- Self-exclusion events
- Limit setting events

#### 2.2.4 Captor Server Specifications (Minimum)
```
CPU: 8+ cores
RAM: 16GB+
Storage: 500GB SSD (for buffering)
Network: 1 Gbps NIC
OS: Linux (Ubuntu 22.04 LTS / RHEL 8 / Oracle Linux)
```

---

### 2.3 Safe Component

#### 2.3.1 Technology Stack (Recommended)
```
- OS: Oracle Linux 8+ / Red Hat Enterprise Linux 8+
- File System: ext4 / XFS
- FTPS Server: vsftpd / ProFTPD with TLS
- Encryption: OpenSSL with X.509 v3 certificates
- Storage: NAS / SAN with RAID for redundancy
- Backup: Tape backup / Cloud backup for 96-month cold storage
- Monitoring: Nagios / Zabbix
```

#### 2.3.2 Safe Functions

1. **Data Reception**
   - Receive data from Captor via secure protocol
   - Validate data integrity
   - Queue for processing

2. **Data Processing**
   ```
   For each data batch:
   1. Sign data with digital signature
   2. Compress data (gzip/bzip2)
   3. Encrypt with Multicert Public Key (X.509 v3, RFC 5280)
   4. Store in SRIJ-defined folder structure
   ```

3. **Storage Management**
   - **Hot Storage (24 months)**: Fast SSD storage for immediate access
   - **Cold Storage (96 months)**: Slower HDD/tape for long-term retention
   - Automated data lifecycle management
   - Migration from hot to cold storage after 24 months

4. **SRIJ Access**
   - FTPS server running 24/7
   - Dedicated credentials for SRIJ
   - Read-only access to data folders
   - Audit logging of SRIJ access

5. **Backup and Redundancy**
   - Daily incremental backups
   - Weekly full backups
   - Off-site backup storage
   - RAID configuration for disk redundancy

#### 2.3.3 Folder Structure Example
```
/safe/
├── player_data/
│   ├── 2025/
│   │   ├── 01/
│   │   │   ├── registrations/
│   │   │   ├── authentications/
│   │   │   └── ...
│   │   ├── 02/
│   │   └── ...
│   └── ...
├── transactions/
│   ├── 2025/
│   │   ├── 01/
│   │   │   ├── deposits/
│   │   │   ├── withdrawals/
│   │   │   └── ...
│   │   └── ...
│   └── ...
├── gaming_data/
│   ├── 2025/
│   │   ├── 01/
│   │   │   ├── bets/
│   │   │   ├── outcomes/
│   │   │   └── ...
│   │   └── ...
│   └── ...
└── ...

Note: Actual structure must follow SRIJ Data Model specification
```

#### 2.3.4 Safe Server Specifications (Minimum)

**Hot Storage Server (24 months)**
```
CPU: 8+ cores
RAM: 32GB+
Storage: 10TB+ SSD (depends on player volume)
Network: 1 Gbps NIC (20 Mbps dedicated to SRIJ minimum)
OS: Oracle Linux 8+ / RHEL 8+
RAID: RAID 10 for performance and redundancy
```

**Cold Storage (96 months)**
```
Storage: 40TB+ HDD/Tape (depends on player volume)
Backup System: Tape library or cloud storage
Redundancy: Geographic redundancy recommended
```

#### 2.3.5 Data Encryption Implementation
```bash
# Example encryption workflow
# 1. Sign data
openssl dgst -sha256 -sign private_key.pem -out data.sig data.json

# 2. Compress
gzip data.json

# 3. Encrypt with Multicert Public Key
openssl smime -encrypt -aes-256-cbc -in data.json.gz \
    -out data.json.gz.enc -outform DER multicert_public_key.pem
```

---

## 3. Network Architecture

### 3.1 Network Diagram

```
                         Internet
                            │
                            │
                ┌───────────┴──────────┐
                │                      │
                │   Firewall / WAF     │
                │   (DDoS Protection)  │
                │                      │
                └───────────┬──────────┘
                            │
            ┌───────────────┼───────────────┐
            │               │               │
            │   DMZ Network (Portuguese DC) │
            │               │               │
            │   ┌───────────▼───────────┐   │
            │   │  Load Balancer        │   │
            │   │  (Gateway Frontend)   │   │
            │   └───────────┬───────────┘   │
            │               │               │
            │   ┌───────────▼───────────┐   │
            │   │  Gateway Servers      │   │
            │   │  (Private Subnet)     │   │
            │   └───────────┬───────────┘   │
            │               │               │
            └───────────────┼───────────────┘
                            │
            ┌───────────────┼───────────────┐
            │               │               │
            │   Application Network         │
            │               │               │
            │   ┌───────────▼───────────┐   │
            │   │  Captor Servers       │   │
            │   └───────────┬───────────┘   │
            │               │               │
            └───────────────┼───────────────┘
                            │
            ┌───────────────┼───────────────┐
            │               │               │
            │   Storage Network             │
            │               │               │
            │   ┌───────────▼───────────┐   │
            │   │  Safe Infrastructure  │   │
            │   │  (FTPS Server)        │   │
            │   └───────────┬───────────┘   │
            │               │               │
            └───────────────┼───────────────┘
                            │
                            │ FTPS (20+ Mbps)
                            ▼
                    ┌───────────────┐
                    │     SRIJ      │
                    │ Infrastructure│
                    └───────────────┘

                            │
                            │ NTP (UDP 123)
                            ▼
                ┌───────────────────────┐
                │ Lisbon Astronomical   │
                │ Observatory NTP       │
                └───────────────────────┘
```

### 3.2 Network Subnets

```
DMZ Network:       10.0.1.0/24
  - Load Balancer: 10.0.1.10
  - Gateway-1:     10.0.1.20
  - Gateway-2:     10.0.1.21

Application Net:   10.0.2.0/24
  - Captor-1:      10.0.2.10
  - Captor-2:      10.0.2.11

Storage Network:   10.0.3.0/24
  - Safe-Primary:  10.0.3.10
  - Safe-Backup:   10.0.3.11

Management Net:    10.0.255.0/24
  - Monitoring:    10.0.255.10
  - Jump Host:     10.0.255.20
```

### 3.3 Firewall Rules

#### 3.3.1 Inbound Rules (DMZ - Load Balancer)
```
Source: Internet (Portuguese IPs preferred)
Destination: Load Balancer Public IP
Port: 443 (HTTPS)
Protocol: TCP
Action: ALLOW
Description: Player traffic to .pt domain

Source: SRIJ Infrastructure IPs
Destination: Safe FTPS Server
Port: 990, 21 (FTPS)
Protocol: TCP
Action: ALLOW
Description: SRIJ data access

Source: Lisbon Observatory NTP
Destination: All servers
Port: 123 (NTP)
Protocol: UDP
Action: ALLOW
Description: Time synchronization
```

#### 3.3.2 Outbound Rules
```
Source: All servers
Destination: Lisbon Observatory NTP
Port: 123 (NTP)
Protocol: UDP
Action: ALLOW
Description: Time synchronization

Source: Gateway Servers
Destination: Gaming Platform
Port: 443 (HTTPS)
Protocol: TCP
Action: ALLOW
Description: Communication with main platform

Source: Captor Servers
Destination: Safe Servers
Port: 990, 21 (FTPS)
Protocol: TCP
Action: ALLOW
Description: Data transfer to Safe

Source: Safe Servers
Destination: SRIJ Infrastructure
Port: 990, 21 (FTPS)
Protocol: TCP
Action: ALLOW
Description: SRIJ data collection
```

#### 3.3.3 Internal Rules
```
Source: Gateway Servers
Destination: Captor Servers
Port: Application-specific (e.g., 8080)
Protocol: TCP
Action: ALLOW
Description: Event data transfer

Source: Management Network
Destination: All Networks
Port: 22 (SSH)
Protocol: TCP
Action: ALLOW
Description: Administrative access
```

---

## 4. Data Flow

### 4.1 Player Session Flow
```
1. Portuguese player navigates to operatorname.pt
2. DNS resolves to Gateway Load Balancer (Portugal)
3. HTTPS connection established to Gateway
4. Gateway verifies:
   - IP is Portuguese OR account registered in Portugal
   - Domain is .pt
5. Gateway logs session initiation event
6. Gateway event sent to Captor
7. Gateway proxies request to main gaming platform
8. Gaming platform processes request
9. Response flows back through Gateway to player
10. Captor categorizes and queues event data
11. Captor transfers event to Safe (secure protocol)
12. Safe processes: sign → compress → encrypt → store
```

### 4.2 Data Storage Flow
```
1. Captor receives gaming event
2. Categorize event per SRIJ Data Model
3. Format data according to SRIJ specifications
4. Transfer to Safe via FTPS/HTTPS
5. Safe receives and validates data
6. Digital signature applied
7. Data compressed (gzip)
8. Data encrypted with Multicert Public Key (X.509 v3)
9. Store in appropriate folder (year/month/category)
10. If data is older than 24 months:
    - Migrate to cold storage
    - Maintain accessibility (slower retrieval acceptable)
```

### 4.3 SRIJ Audit Flow
```
1. SRIJ initiates FTPS connection to Safe
2. Safe authenticates SRIJ credentials
3. SRIJ navigates folder structure
4. SRIJ downloads required data files
5. SRIJ performs analysis/audit
6. Safe logs all SRIJ access for compliance
```

---

## 5. Hosting Requirements

### 5.1 Data Center Location
**MANDATORY: Portugal**

Recommended locations:
- Lisbon (primary)
- Porto (alternative/DR site)
- Coimbra (alternative)

### 5.2 Data Center Requirements

#### 5.2.1 Tier Classification
- **Minimum**: Tier II
- **Recommended**: Tier III
- **Ideal**: Tier IV

#### 5.2.2 Certifications
- ISO 27001 (Information Security)
- ISO 9001 (Quality Management)
- PCI DSS (if handling payments)
- SOC 2 Type II

#### 5.2.3 Connectivity
- Multiple upstream providers (redundancy)
- Minimum 1 Gbps connectivity
- 20 Mbps guaranteed bandwidth to SRIJ
- Low latency to SRIJ infrastructure
- Direct peering with major Portuguese ISPs

#### 5.2.4 Power and Cooling
- Redundant power feeds
- UPS systems
- Backup generators
- N+1 cooling redundancy

#### 5.2.5 Physical Security
- 24/7 security personnel
- Biometric access control
- CCTV surveillance
- Mantrap entry systems

#### 5.2.6 Support
- 24/7/365 on-site support
- Portuguese-speaking staff
- SLA with guaranteed uptime (99.95%+)

### 5.3 Potential Portuguese Data Center Providers

Research and contact these providers:
1. **Equinix (Lisbon)**
   - International provider with presence in Portugal
   - Multiple data centers

2. **Lusavouga**
   - Portuguese provider
   - Data centers in Lisbon

3. **PTIN (Portugal Telecom)**
   - National telecommunications provider
   - Data center services

4. **Claranet Portugal**
   - Data center and cloud services
   - Portuguese presence

5. **Vodafone Portugal Data Centers**
   - Telecommunications infrastructure
   - Enterprise hosting

**IMPORTANT**: Contact SRIJ to confirm if they have a preferred or certified list of data center providers.

---

## 6. Time Synchronization Architecture

### 6.1 NTP Configuration

All systems must synchronize with Lisbon Astronomical Observatory NTP servers.

#### 6.1.1 NTP Server Configuration (on each server)
```bash
# /etc/ntp.conf or /etc/chrony/chrony.conf

# Primary NTP servers (Lisbon Astronomical Observatory)
# Note: Contact SRIJ for exact NTP server addresses
server ntp.oal.ul.pt iburst
server ntp1.oal.ul.pt iburst
server ntp2.oal.ul.pt iburst

# Fallback to Portuguese pool
server 0.pt.pool.ntp.org iburst
server 1.pt.pool.ntp.org iburst

# Stratum configuration
stratumweight 0

# Drift file
driftfile /var/lib/ntp/drift
```

#### 6.1.2 NTP Monitoring
- Monitor time synchronization status
- Alert if drift exceeds thresholds
- Log all time adjustments
- Verify synchronization with SRIJ requirements

---

## 7. Security Architecture

### 7.1 Defense in Depth

```
Layer 1: Network Perimeter
  - DDoS protection (Cloudflare / Akamai / local)
  - Web Application Firewall (WAF)
  - Geographic filtering (Portuguese IPs prioritized)

Layer 2: Network Security
  - Firewall rules (as specified above)
  - Network segmentation (DMZ, App, Storage, Mgmt)
  - IDS/IPS (Snort / Suricata)
  - VPN for administrative access

Layer 3: Host Security
  - OS hardening (CIS benchmarks)
  - Host-based firewall (iptables / firewalld)
  - Antivirus / Anti-malware
  - File integrity monitoring (AIDE / Tripwire)
  - Security updates and patching

Layer 4: Application Security
  - Secure coding practices
  - Input validation
  - Output encoding
  - Authentication and authorization
  - Session management
  - API security

Layer 5: Data Security
  - Encryption at rest (Safe storage)
  - Encryption in transit (TLS/FTPS)
  - Data signing (integrity)
  - Access controls
  - Audit logging
```

### 7.2 Certificate Management

#### 7.2.1 SSL/TLS Certificates
```
.pt Domain: Commercial SSL certificate
  - Issuer: Trusted CA (DigiCert, GlobalSign, etc.)
  - Type: EV SSL recommended for trust
  - Validity: Annual renewal

Internal Communications: Internal CA
  - Gateway ↔ Platform: TLS 1.3
  - Captor ↔ Safe: TLS 1.3 or FTPS
```

#### 7.2.2 SRIJ Multicert Public Key
```
Purpose: Encrypting data for SRIJ
Format: X.509 v3 certificate
Standard: RFC 5280
Source: Provided by SRIJ
Usage: All data stored in Safe must be encrypted with this key
```

### 7.3 Access Control

#### 7.3.1 Administrative Access
```
Method: SSH with key-based authentication
MFA: Required for all administrative access
Jump Host: Bastion host for all SSH access
Privilege Escalation: sudo with logging
Session Recording: All administrative sessions recorded
```

#### 7.3.2 SRIJ Access
```
Method: FTPS with dedicated credentials
Access Level: Read-only to Safe data
Audit: All SRIJ access logged
Authentication: Username/password + IP whitelist
```

#### 7.3.3 Application Access
```
Gateway → Platform: API key + mTLS
Captor → Safe: Service account + certificate authentication
Monitoring → Servers: Read-only service accounts
```

---

## 8. Monitoring and Alerting

### 8.1 Infrastructure Monitoring

```
Tool: Prometheus + Grafana / Zabbix / Nagios

Metrics to Monitor:
- CPU usage (alert > 80%)
- Memory usage (alert > 90%)
- Disk usage (alert > 85% hot storage, > 95% cold storage)
- Network bandwidth utilization
- Packet loss and latency
- Service availability (Gateway, Captor, Safe, FTPS)
- Process health checks

Alerts:
- Service down
- High resource usage
- Disk space critical
- Network connectivity issues
- SRIJ FTPS access failures
```

### 8.2 Application Monitoring

```
Tool: ELK Stack / Splunk / DataDog

Logs to Collect:
- Gateway access logs
- Application error logs
- Captor processing logs
- Safe storage logs
- FTPS access logs
- Security events

Metrics:
- Request rate
- Response time
- Error rate
- Player session counts
- Event capture rate
- Data transfer rate to Safe
```

### 8.3 Security Monitoring

```
Tool: SIEM (Security Information and Event Management)
Options: Splunk / ELK + Wazuh / IBM QRadar

Events to Monitor:
- Failed login attempts
- Privilege escalation
- File modifications (especially in Safe)
- Network anomalies
- IDS/IPS alerts
- Firewall blocks
- Certificate expiration warnings

Alerts:
- Potential intrusion attempts
- Unauthorized access attempts
- Data exfiltration patterns
- Malware detection
```

### 8.4 Compliance Monitoring

```
Downtime Tracking:
- Monitor combined Captor/Safe uptime
- Alert if approaching 4-hour monthly limit
- Dashboard showing current month downtime

Data Retention:
- Monitor storage capacity
- Alert if approaching limits
- Verify 24-month data is on hot storage
- Verify 96-month data is archived

SRIJ Access:
- Log all SRIJ FTPS connections
- Monitor data downloads by SRIJ
- Report on SRIJ audit activities
```

---

## 9. Disaster Recovery and Business Continuity

### 9.1 Backup Strategy

#### 9.1.1 Gateway Backup
```
Configuration: Daily backup
Data: Logs backed up before rotation
Recovery Time Objective (RTO): 4 hours
Recovery Point Objective (RPO): 24 hours
```

#### 9.1.2 Captor Backup
```
Configuration: Daily backup
Buffered Data: Continuous backup to Safe
RTO: 4 hours
RPO: 1 hour (buffered events)
```

#### 9.1.3 Safe Backup
```
Hot Storage (24 months):
  - Method: Continuous replication to backup Safe
  - Frequency: Real-time or hourly sync
  - Location: Secondary Portuguese data center
  - RTO: 4 hours
  - RPO: 1 hour

Cold Storage (96 months):
  - Method: Tape backup or cloud backup
  - Frequency: Monthly full backup
  - Location: Off-site Portuguese facility
  - RTO: 1 week (per SRIJ requirements)
  - RPO: 1 month
```

### 9.2 Disaster Recovery Plan

#### 9.2.1 DR Site
```
Location: Secondary Portuguese data center
  - Lisbon → Porto (or vice versa)
  - Maintain standby infrastructure
  - Automated failover for critical components

Components:
  - Standby Gateway servers
  - Standby Captor servers
  - Replica Safe (hot storage)
  - Network connectivity to SRIJ
```

#### 9.2.2 Failover Procedures
```
Gateway Failover:
1. DNS failover to DR site (automated)
2. Activate standby Gateway servers
3. Verify player connectivity
4. Notify SRIJ of DR activation
Time: < 1 hour

Captor Failover:
1. Redirect Gateway to DR Captor
2. Activate standby Captor
3. Resume event processing
4. Verify data flow to Safe
Time: < 2 hours

Safe Failover:
1. Activate replica Safe
2. Update FTPS access for SRIJ
3. Verify data integrity
4. Resume normal operations
Time: < 4 hours (within monthly downtime limit)
```

#### 9.2.3 Recovery Scenarios

**Scenario 1: Server Failure**
```
Impact: Single server failure
Action: Automatic failover to redundant server
RTO: < 15 minutes (automated)
```

**Scenario 2: Network Failure**
```
Impact: Loss of connectivity
Action: Failover to backup network provider
RTO: < 30 minutes
```

**Scenario 3: Data Center Failure**
```
Impact: Complete DC outage
Action: Failover to DR site
RTO: < 4 hours (per compliance requirements)
Notification: Immediate notification to SRIJ
```

**Scenario 4: Data Loss**
```
Impact: Corruption or loss of Safe data
Action: Restore from backup
RTO: < 1 week (per SRIJ requirements for data recovery)
Verification: Data integrity verification with SRIJ
```

### 9.3 Business Continuity Plan

```
Objective: Resume operations within 1 month of major disaster

Components:
1. Documented procedures for all scenarios
2. Contact lists (internal team, SRIJ, vendors)
3. Regular DR drills (quarterly)
4. Incident response team
5. Communication plan (to SRIJ, players, stakeholders)

Testing:
- Monthly: Backup restoration test
- Quarterly: DR failover test
- Annually: Full disaster scenario simulation
```

---

## 10. Scalability and Performance

### 10.1 Scaling Strategy

#### 10.1.1 Horizontal Scaling
```
Gateway: Scale out with additional servers behind load balancer
  - Add servers during high traffic periods
  - Auto-scaling based on CPU/memory metrics

Captor: Parallel processing with multiple instances
  - Event partitioning (by player ID hash)
  - Load balancing across Captor instances

Safe: Scale storage capacity
  - Add storage volumes as data grows
  - Maintain 24-month hot storage capacity
```

#### 10.1.2 Performance Optimization
```
Gateway:
  - CDN for static assets (if applicable)
  - Connection pooling to platform
  - Caching (where appropriate)
  - HTTP/2 or HTTP/3

Captor:
  - Asynchronous event processing
  - Batch processing where allowed
  - Message queue for buffering
  - Efficient data serialization

Safe:
  - Fast SSD for hot storage
  - Indexed directory structure
  - Optimized compression algorithms
  - Parallel encryption processing
```

### 10.2 Capacity Planning

#### 10.2.1 Calculate Storage Requirements
```
Assumptions:
- Average events per player per day: 1,000
- Average event size: 500 bytes
- Number of active players: 10,000
- Compression ratio: 5:1

Daily data volume:
10,000 players × 1,000 events × 500 bytes = 5 GB raw
5 GB / 5 (compression) = 1 GB compressed per day

Monthly: 30 GB
24 months (hot): 720 GB → Provision 2 TB SSD (growth buffer)
96 months (cold): 2.88 TB → Provision 10 TB HDD (growth buffer)

Scale accordingly for your expected player volume.
```

#### 10.2.2 Network Capacity
```
Peak hour assumptions:
- 50% of daily events in 4-hour peak window
- 5 GB / 2 = 2.5 GB in 4 hours = 625 MB/hour = 10.4 MB/minute

Peak bandwidth to Safe:
10.4 MB/min × 8 bits/MB = 83 Mbps peak

Recommendation: 200 Mbps internal network capacity
SRIJ connection: 20 Mbps (regulatory minimum) → 50 Mbps (buffer)
```

---

## 11. Development and Testing

### 11.1 Development Environments

```
Local Development:
  - Docker containers simulating Gateway, Captor, Safe
  - Mock SRIJ FTPS server
  - Test data generators

Staging Environment (in Portugal):
  - Mirror of production architecture
  - Lower capacity (50% of production)
  - Connected to SRIJ test environment (if available)
  - Full integration testing

Production:
  - Full infrastructure as documented
  - Connected to live SRIJ
```

### 11.2 Testing Requirements

#### 11.2.1 Functional Testing
```
- Gateway routing and geolocation
- Captor event capture and categorization
- Safe encryption, compression, signing
- FTPS access from SRIJ
- Data integrity verification
- Time synchronization
```

#### 11.2.2 Performance Testing
```
- Load testing (expected player volumes × 2)
- Stress testing (peak event rates)
- Captor processing speed verification
- Safe write throughput
- Network bandwidth testing
```

#### 11.2.3 Security Testing
```
- Penetration testing
- Vulnerability scanning
- Encryption validation
- Access control testing
- Firewall rule verification
```

#### 11.2.4 Compliance Testing
```
- SRIJ homologation testing
- Independent lab certification
- Data format verification
- Audit trail testing
- Downtime measurement
```

#### 11.2.5 Disaster Recovery Testing
```
- Backup restoration
- Failover procedures
- Data recovery within 1 week
- Business continuity plan execution
```

---

## 12. Deployment Timeline

### 12.1 Suggested Project Phases

```
Phase 1: Planning and Design (4 weeks)
  - Finalize architecture
  - Select data center
  - Engage SRIJ for technical specifications
  - Engage independent testing laboratory

Phase 2: Infrastructure Setup (6 weeks)
  - Data center provisioning
  - Network configuration
  - Server installation and OS hardening
  - Firewall and security setup
  - FTPS server configuration

Phase 3: Application Development (12 weeks)
  - Gateway development
  - Captor development
  - Safe integration
  - Monitoring and logging setup
  - Admin interfaces

Phase 4: Integration (4 weeks)
  - Integrate Gateway with main platform
  - Integrate Captor with Gateway
  - Integrate Safe with Captor
  - SRIJ connectivity setup
  - NTP synchronization

Phase 5: Testing (8 weeks)
  - Unit testing
  - Integration testing
  - Performance testing
  - Security testing
  - Independent lab certification

Phase 6: SRIJ Homologation (6-8 weeks)
  - Submit to SRIJ testing
  - Address SRIJ feedback
  - Re-testing as needed
  - Obtain approval

Phase 7: Pilot Launch (4 weeks)
  - Limited player access
  - Monitor all systems
  - SRIJ observation period
  - Fine-tuning

Phase 8: Full Production (Ongoing)
  - Full player access
  - Continuous monitoring
  - Regular reporting to SRIJ
  - Ongoing compliance

Total Estimated Timeline: 44-46 weeks (~11 months)
```

---

## 13. Cost Considerations

### 13.1 Infrastructure Costs (Annual Estimates)

```
Data Center Hosting (Portugal):
  - Rack space, power, network: €30,000 - €60,000

Servers and Storage:
  - Gateway servers (2×): €10,000
  - Captor servers (2×): €15,000
  - Safe servers (2×): €20,000
  - Storage (24-month hot): €15,000
  - Storage (96-month cold): €10,000
  - Total hardware: €70,000 (one-time or leased)

Network:
  - Bandwidth: €6,000 - €12,000
  - SRIJ dedicated connection: Included or €2,000

Software Licenses:
  - OS licenses (if RHEL): €5,000
  - Monitoring tools: €3,000
  - Security tools: €5,000

Personnel:
  - DevOps engineers (2×): €120,000
  - System administrators: €60,000
  - Security specialist: €60,000

Third-party Services:
  - Independent testing lab: €50,000 (one-time)
  - Ongoing audits: €10,000 annually
  - Consulting: €20,000

Financial Guarantees:
  - Player liability: €500,000 (held)
  - Tax payment: €100,000 (held)

Annual Recurring Costs: ~€250,000 - €350,000
One-time Setup Costs: ~€150,000 - €200,000

Note: Costs vary based on scale and vendor selection
```

---

## 14. Next Steps

### 14.1 Immediate Actions

1. **Contact SRIJ**
   - Request technical documentation
   - Obtain Data Model specifications
   - Get FTPS connection details
   - Inquire about homologation process timeline
   - Request list of approved testing laboratories

2. **Select Data Center**
   - RFP to Portuguese data center providers
   - Site visits
   - Evaluate connectivity to SRIJ
   - Negotiate SLA and pricing

3. **Assemble Team**
   - Hire or contract technical team
   - Engage project manager
   - Identify legal counsel (Portuguese gambling law)
   - Select independent testing laboratory

4. **Obtain Multicert Public Key**
   - Request from SRIJ
   - Implement in Safe encryption workflow

5. **Design Detailed Architecture**
   - Customize this architecture for your specific needs
   - Define exact server specifications
   - Create network diagrams
   - Document all procedures

### 14.2 Regulatory Compliance Checklist

- [ ] SRIJ license application submitted
- [ ] €500,000 player liability guarantee arranged
- [ ] €100,000 tax payment guarantee arranged
- [ ] .pt domain registered
- [ ] Independent testing lab engaged
- [ ] Data center in Portugal contracted
- [ ] SRIJ technical documentation obtained
- [ ] Gateway infrastructure deployed in Portugal
- [ ] Safe infrastructure deployed in Portugal
- [ ] FTPS access configured for SRIJ
- [ ] NTP synchronization configured
- [ ] Data encryption with Multicert key implemented
- [ ] Data Model folder structure implemented
- [ ] 10-year data retention implemented
- [ ] 24-month hot storage verified
- [ ] Captor processing at platform speed verified
- [ ] Maximum 4-hour monthly downtime monitoring in place
- [ ] Disaster recovery plan tested
- [ ] Independent lab certification obtained
- [ ] SRIJ homologation testing completed
- [ ] SRIJ approval received
- [ ] Monthly reporting to SRIJ established

---

## Document Information

**Version**: 1.0
**Last Updated**: 2025-11-10
**Author**: Technical Architecture Team
**Status**: Draft for Implementation

---

**IMPORTANT**: This architecture is based on publicly available SRIJ requirements. Always validate all technical specifications with SRIJ directly and obtain official documentation before implementation. Requirements may change, and specific details may require consultation with SRIJ technical staff.
