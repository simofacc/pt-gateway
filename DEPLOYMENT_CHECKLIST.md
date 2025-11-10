# SRIJ ERI Gateway - Complete Deployment Checklist

## Overview

This comprehensive checklist guides you through the complete deployment of a SRIJ-compliant Entry and Registry Infrastructure (ERI) for online gambling operations in Portugal.

**Timeline**: Approximately 11-12 months from start to production
**Complexity**: High
**Regulatory Oversight**: SRIJ (Serviço de Regulação e Inspeção de Jogos)

---

## Phase 1: Pre-Planning and Regulatory (Weeks 1-4)

### 1.1 Regulatory Engagement

- [ ] **Contact SRIJ**
  - [ ] Request information package for new operators
  - [ ] Obtain license application forms
  - [ ] Request list of recognized independent testing laboratories
  - [ ] Request technical documentation (Portuguese/English)
  - [ ] Obtain SRIJ Data Model specifications
  - [ ] Request Homologation Manual
  - [ ] Obtain Multicert Public Key for encryption
  - [ ] Get SRIJ infrastructure connection details (IP addresses, ports)
  - [ ] Ask for preferred/approved data center providers
  - [ ] Clarify timeline expectations

**SRIJ Contact**:
- Website: https://www.srij.turismodeportugal.pt
- Address: Turismo de Portugal, Rua Ivone Silva, Lote 6, 1050-124 Lisboa, Portugal

---

### 1.2 Financial Preparation

- [ ] **Arrange Financial Guarantees**
  - [ ] €500,000 player liability guarantee (bank deposit, insurance, or surety)
  - [ ] €100,000 IEJO tax payment guarantee
  - [ ] Contact Portuguese banks for guarantee arrangements
  - [ ] Prepare financial statements and proof of funds

---

### 1.3 Legal and Corporate

- [ ] **Legal Setup**
  - [ ] Engage Portuguese legal counsel (gambling law specialist)
  - [ ] Register Portuguese business entity (if required)
  - [ ] Obtain Portuguese tax identification number (NIF)
  - [ ] Review all Portuguese gambling regulations
  - [ ] Prepare corporate documentation for license application
  - [ ] Background checks for key personnel (directors, shareholders)
  - [ ] Prepare anti-money laundering (AML) policies
  - [ ] Prepare responsible gambling policies

---

### 1.4 Domain Registration

- [ ] **.pt Domain**
  - [ ] Choose domain name: _____________.pt
  - [ ] Register with Portuguese domain registrar (DNS.pt)
  - [ ] Provide required documentation (Portuguese NIF, etc.)
  - [ ] Configure DNS with registrar
  - [ ] Set up domain email addresses

---

### 1.5 Team Assembly

- [ ] **Hire/Contract Key Personnel**
  - [ ] Project manager (gambling tech experience)
  - [ ] DevOps engineers (2+)
  - [ ] Backend developers (2+)
  - [ ] Security specialist (InfoSec/SRIJ compliance)
  - [ ] System administrators (Portuguese data center experience)
  - [ ] QA/Testing engineers
  - [ ] Compliance officer
  - [ ] Portuguese-speaking liaison (SRIJ communication)

---

### 1.6 Testing Laboratory

- [ ] **Engage Independent Testing Lab**
  - [ ] Request list of SRIJ-recognized laboratories
  - [ ] Get quotes from minimum 2 labs
  - [ ] Select laboratory
  - [ ] Sign contract
  - [ ] Establish communication channel
  - [ ] Obtain testing requirements and procedures
  - [ ] Schedule preliminary consultation

**Common Labs** (verify SRIJ recognition):
- eCOGRA
- GLI (Gaming Laboratories International)
- iTech Labs
- BMM Testlabs
- SIQ (Slovenia - European gaming testing)

---

## Phase 2: Infrastructure Planning (Weeks 5-8)

### 2.1 Architecture Design

- [ ] **Finalize Technical Architecture**
  - [ ] Review TECHNICAL_ARCHITECTURE.md document
  - [ ] Customize for your specific player volume and requirements
  - [ ] Create detailed network diagrams
  - [ ] Define server specifications
  - [ ] Design data flow
  - [ ] Plan security architecture
  - [ ] Design monitoring and alerting strategy
  - [ ] Plan disaster recovery architecture

---

### 2.2 Data Center Selection

- [ ] **Select Primary Data Center (Portugal)**
  - [ ] Request quotes from providers:
    - [ ] Equinix Lisbon
    - [ ] Interxion Lisbon
    - [ ] Claranet Portugal
    - [ ] PTIN
    - [ ] Vodafone Portugal
    - [ ] Other: _______________
  - [ ] Test network latency to SRIJ infrastructure
  - [ ] Verify ISO 27001 certification
  - [ ] Review SLA terms (target: 99.95%+ uptime)
  - [ ] Schedule site visits to top 2-3 finalists
  - [ ] Conduct site inspections
  - [ ] Check references
  - [ ] Negotiate contract
  - [ ] Sign colocation/hosting agreement
  - [ ] Pay deposit/setup fees

**Selected Primary Data Center**: _______________
**Location**: _______________
**Contract Start Date**: _______________

---

- [ ] **Select Disaster Recovery Data Center (Portugal)**
  - [ ] Identify DR location (Porto or secondary Lisbon facility)
  - [ ] Request quotes
  - [ ] Verify geographic diversity from primary
  - [ ] Confirm SRIJ connectivity available
  - [ ] Sign DR hosting agreement

**Selected DR Data Center**: _______________
**Location**: _______________

---

### 2.3 Connectivity Planning

- [ ] **Network Connectivity**
  - [ ] Order primary internet connectivity (1+ Gbps)
  - [ ] Order SRIJ dedicated connection (20+ Mbps)
  - [ ] Order NTP connectivity (Lisbon Astronomical Observatory)
  - [ ] Plan VPN/secure tunnels to main gaming platform (if external)
  - [ ] Order cross-connects (if in carrier-neutral facility)
  - [ ] Obtain IP address allocations
  - [ ] Plan BGP routing (if using BYOIP)

**Primary Internet Provider**: _______________
**SRIJ Connection Details**: _______________

---

### 2.4 Hardware Procurement

- [ ] **Order Servers and Equipment**

**Gateway Servers (Primary Site)**:
- [ ] 2× Gateway servers (specs: 4+ cores, 8GB+ RAM, 100GB SSD)
  - Vendor: _______________
  - Model: _______________
  - Order date: _______________
  - Expected delivery: _______________

**Captor Servers (Primary Site)**:
- [ ] 2× Captor servers (specs: 8+ cores, 16GB+ RAM, 500GB SSD)
  - Vendor: _______________
  - Model: _______________

**Safe Servers (Primary Site)**:
- [ ] 2× Safe servers (specs: 8+ cores, 32GB+ RAM, 10TB+ storage, RAID 10)
  - Vendor: _______________
  - Model: _______________

**Network Equipment**:
- [ ] Firewalls (2× for redundancy)
- [ ] Switches (2× for redundancy)
- [ ] Load balancer (or software-based)

**DR Site Equipment**:
- [ ] 1× Gateway server (standby)
- [ ] 1× Captor server (standby)
- [ ] 1× Safe server (replica)
- [ ] Network equipment

**Additional**:
- [ ] KVM switches
- [ ] PDUs (power distribution units)
- [ ] Cables, rails, accessories

---

### 2.5 Software Licensing

- [ ] **Operating Systems**
  - [ ] Oracle Linux or Red Hat Enterprise Linux licenses (if commercial)
  - [ ] Calculate number of licenses needed
  - [ ] Purchase licenses

- [ ] **Security Software**
  - [ ] Antivirus/anti-malware
  - [ ] IDS/IPS (if commercial solution)
  - [ ] SIEM (if commercial)

- [ ] **Monitoring Tools**
  - [ ] Prometheus/Grafana (open source) OR
  - [ ] Commercial monitoring (Datadog, New Relic, etc.)

- [ ] **Backup Software**
  - [ ] Backup solution for Safe data

---

## Phase 3: Infrastructure Deployment (Weeks 9-14)

### 3.1 Data Center Setup

- [ ] **Primary Site - Physical Setup**
  - [ ] Coordinate hardware delivery to data center
  - [ ] Schedule rack installation with data center
  - [ ] Install servers in racks
  - [ ] Connect power (redundant feeds)
  - [ ] Connect network cables
  - [ ] Test remote console access (IPMI/iLO/iDRAC)
  - [ ] Label all equipment and cables
  - [ ] Document rack layout

---

### 3.2 Operating System Installation

- [ ] **Gateway Servers**
  - [ ] Install Oracle Linux / RHEL on Gateway-1
  - [ ] Install Oracle Linux / RHEL on Gateway-2
  - [ ] Apply OS updates
  - [ ] Configure hostnames and network interfaces
  - [ ] Configure NTP (Lisbon Astronomical Observatory)
  - [ ] Harden OS (CIS benchmarks)
  - [ ] Install base monitoring agents

- [ ] **Captor Servers**
  - [ ] Install OS on Captor-1
  - [ ] Install OS on Captor-2
  - [ ] Apply updates and hardening
  - [ ] Configure NTP

- [ ] **Safe Servers**
  - [ ] Install OS on Safe-1
  - [ ] Install OS on Safe-2
  - [ ] Configure RAID arrays
  - [ ] Create file systems (ext4/XFS)
  - [ ] Apply updates and hardening
  - [ ] Configure NTP

---

### 3.3 Network Configuration

- [ ] **Firewall Setup**
  - [ ] Install and configure firewall appliances/software
  - [ ] Define network zones (DMZ, App, Storage, Mgmt)
  - [ ] Implement firewall rules per TECHNICAL_ARCHITECTURE.md
  - [ ] Enable logging
  - [ ] Test failover (if redundant firewalls)

- [ ] **Network Segmentation**
  - [ ] Configure VLANs:
    - [ ] DMZ Network (10.0.1.0/24)
    - [ ] Application Network (10.0.2.0/24)
    - [ ] Storage Network (10.0.3.0/24)
    - [ ] Management Network (10.0.255.0/24)
  - [ ] Configure inter-VLAN routing
  - [ ] Test network connectivity

- [ ] **SRIJ Connectivity**
  - [ ] Configure dedicated SRIJ network path
  - [ ] Test connectivity to SRIJ IP addresses
  - [ ] Verify bandwidth (minimum 20 Mbps)
  - [ ] Measure latency to SRIJ

- [ ] **Load Balancer**
  - [ ] Install/configure load balancer (HAProxy/Nginx/hardware)
  - [ ] Configure .pt domain SSL termination
  - [ ] Set up backend server pools (Gateway servers)
  - [ ] Configure health checks
  - [ ] Test failover

---

### 3.4 Security Implementation

- [ ] **SSL/TLS Certificates**
  - [ ] Purchase SSL certificate for .pt domain (EV SSL recommended)
  - [ ] Install on load balancer
  - [ ] Configure TLS 1.3
  - [ ] Generate internal certificates for inter-component communication
  - [ ] Set up certificate renewal process

- [ ] **Encryption Setup**
  - [ ] Obtain Multicert Public Key from SRIJ
  - [ ] Install on Safe servers
  - [ ] Test encryption workflow (sign → compress → encrypt)
  - [ ] Verify X.509 v3 and RFC 5280 compliance

- [ ] **Access Control**
  - [ ] Create admin user accounts
  - [ ] Set up SSH key-based authentication
  - [ ] Disable password authentication
  - [ ] Configure sudo access
  - [ ] Set up jump host/bastion
  - [ ] Enable session recording
  - [ ] Create service accounts (Captor, monitoring, etc.)

- [ ] **Security Tools**
  - [ ] Install and configure antivirus
  - [ ] Set up IDS/IPS (Snort/Suricata)
  - [ ] Configure file integrity monitoring (AIDE/Tripwire)
  - [ ] Set up SIEM (if applicable)
  - [ ] Enable audit logging (auditd)

---

### 3.5 Time Synchronization

- [ ] **NTP Configuration**
  - [ ] Configure all servers to sync with Lisbon Astronomical Observatory NTP
  - [ ] Primary NTP server: ntp.oal.ul.pt (verify address with SRIJ)
  - [ ] Fallback: Portuguese NTP pool servers
  - [ ] Test time synchronization
  - [ ] Monitor time drift
  - [ ] Set up alerts for sync failures

---

## Phase 4: Application Development (Weeks 5-16, parallel with Phase 3)

### 4.1 Gateway Development

- [ ] **Gateway Application**
  - [ ] Choose technology stack (Node.js/Python/Java/Go)
  - [ ] Develop IP geolocation detection module
    - [ ] Integrate MaxMind GeoIP2 or IP2Location
    - [ ] Detect Portuguese IPs
  - [ ] Develop .pt domain enforcement
  - [ ] Develop traffic routing to main gaming platform
  - [ ] Implement session management
  - [ ] Implement logging (all player activities)
  - [ ] Develop event emission to Captor
  - [ ] Create admin interfaces
  - [ ] Write unit tests
  - [ ] Code review

---

### 4.2 Captor Development

- [ ] **Captor Application**
  - [ ] Choose technology stack (Java/Python/Go)
  - [ ] Develop event receiver (from Gateway)
  - [ ] Implement SRIJ Data Model categorization:
    - [ ] Player registration events
    - [ ] Authentication events
    - [ ] Transaction events (deposits/withdrawals)
    - [ ] Betting events (stakes, odds, outcomes)
    - [ ] Game session events
    - [ ] Balance change events
    - [ ] Bonus/promotion events
    - [ ] Self-exclusion events
    - [ ] Limit setting events
  - [ ] Implement message queue (Kafka/RabbitMQ)
  - [ ] Develop data formatting per SRIJ specifications
  - [ ] Implement secure transfer to Safe (FTPS/HTTPS)
  - [ ] Develop retry logic
  - [ ] Implement buffering for network issues
  - [ ] Performance optimization (process at platform speed)
  - [ ] Write unit and integration tests

---

### 4.3 Safe Implementation

- [ ] **Safe Infrastructure**
  - [ ] Design and create SRIJ folder structure:
    ```
    /safe/
    ├── player_data/YYYY/MM/[categories]/
    ├── transactions/YYYY/MM/[categories]/
    ├── gaming_data/YYYY/MM/[categories]/
    └── ...
    ```
  - [ ] Develop data reception module
  - [ ] Implement data signing (digital signature)
  - [ ] Implement compression (gzip/bzip2)
  - [ ] Implement encryption (OpenSSL, Multicert Public Key, X.509 v3)
  - [ ] Develop storage management:
    - [ ] Hot storage (24 months) on SSD
    - [ ] Cold storage migration (after 24 months) to HDD/tape
  - [ ] Configure FTPS server for SRIJ access:
    - [ ] Install vsftpd or ProFTPD with TLS
    - [ ] Create SRIJ user account (read-only)
    - [ ] Configure TLS/SSL certificates
    - [ ] Test FTPS connection
  - [ ] Implement data lifecycle management
  - [ ] Implement backup procedures
  - [ ] Set up integrity checking
  - [ ] Write tests

---

### 4.4 Integration

- [ ] **Component Integration**
  - [ ] Integrate Gateway with main gaming platform
    - [ ] API authentication
    - [ ] Request/response handling
    - [ ] Error handling
  - [ ] Integrate Gateway with Captor
    - [ ] Event streaming
    - [ ] Real-time data transfer
  - [ ] Integrate Captor with Safe
    - [ ] Secure protocol (FTPS/HTTPS)
    - [ ] Data transfer testing
    - [ ] Verify data integrity
  - [ ] End-to-end testing (player action → Gateway → Platform → Captor → Safe)

---

### 4.5 Admin Tools

- [ ] **Administrative Interfaces**
  - [ ] Dashboard for system monitoring
  - [ ] SRIJ data access logs viewer
  - [ ] Manual data export tool (for audits)
  - [ ] Configuration management interface
  - [ ] User management (if applicable)
  - [ ] Reporting tools (monthly SRIJ reports)

---

## Phase 5: Monitoring and Logging (Weeks 13-16)

### 5.1 Infrastructure Monitoring

- [ ] **Prometheus + Grafana Setup** (or alternative)
  - [ ] Install Prometheus on monitoring server
  - [ ] Install Grafana
  - [ ] Configure node exporters on all servers
  - [ ] Create dashboards:
    - [ ] System metrics (CPU, memory, disk, network)
    - [ ] Application metrics (request rate, latency, errors)
    - [ ] Downtime tracking (CRITICAL: <4 hours/month limit)
  - [ ] Configure alerts:
    - [ ] Service down
    - [ ] High resource usage
    - [ ] Disk space critical
    - [ ] Approaching monthly downtime limit

---

### 5.2 Application Logging

- [ ] **ELK Stack Setup** (or alternative: Splunk, Graylog)
  - [ ] Install Elasticsearch cluster
  - [ ] Install Logstash
  - [ ] Install Kibana
  - [ ] Configure log shipping from all servers
  - [ ] Create log parsing rules
  - [ ] Create dashboards:
    - [ ] Gateway access logs
    - [ ] Captor processing logs
    - [ ] Safe storage logs
    - [ ] SRIJ FTPS access logs
    - [ ] Security events
  - [ ] Set up log retention (minimum 10 years per SRIJ)

---

### 5.3 Security Monitoring

- [ ] **SIEM Configuration**
  - [ ] Ingest logs from all sources
  - [ ] Create correlation rules:
    - [ ] Failed login attempts
    - [ ] Privilege escalation
    - [ ] File modifications in Safe
    - [ ] Network anomalies
    - [ ] IDS/IPS alerts
  - [ ] Set up alerts
  - [ ] Create incident response playbooks

---

### 5.4 Compliance Monitoring

- [ ] **SRIJ Compliance Dashboards**
  - [ ] Downtime tracking (real-time, monthly aggregate)
  - [ ] Data retention verification
  - [ ] SRIJ access audit trail
  - [ ] Data transfer rates to Safe
  - [ ] Monthly report generator

---

## Phase 6: Testing (Weeks 17-24)

### 6.1 Unit Testing

- [ ] **Component Testing**
  - [ ] Gateway unit tests (>80% code coverage target)
  - [ ] Captor unit tests
  - [ ] Safe unit tests
  - [ ] Test all error handling
  - [ ] Test edge cases

---

### 6.2 Integration Testing

- [ ] **End-to-End Testing**
  - [ ] Player session flow (Portuguese IP → Gateway → Platform)
  - [ ] Event capture flow (Platform → Gateway → Captor → Safe)
  - [ ] Data encryption workflow
  - [ ] SRIJ FTPS access
  - [ ] Time synchronization
  - [ ] Failover scenarios (server failures)
  - [ ] Network failure scenarios

---

### 6.3 Performance Testing

- [ ] **Load Testing**
  - [ ] Define expected player volumes
  - [ ] Simulate 2× expected peak load
  - [ ] Test Gateway throughput
  - [ ] Test Captor processing speed (must match platform speed)
  - [ ] Test Safe write performance
  - [ ] Identify bottlenecks
  - [ ] Optimize as needed

- [ ] **Stress Testing**
  - [ ] Simulate extreme load (e.g., major sporting event)
  - [ ] Test system limits
  - [ ] Verify graceful degradation
  - [ ] Test recovery after overload

---

### 6.4 Security Testing

- [ ] **Vulnerability Assessment**
  - [ ] Run vulnerability scanners (Nessus, OpenVAS)
  - [ ] Patch identified vulnerabilities
  - [ ] Re-scan to verify fixes

- [ ] **Penetration Testing**
  - [ ] Engage third-party penetration testing firm
  - [ ] Test external attack surface
  - [ ] Test internal network segmentation
  - [ ] Test access controls
  - [ ] Remediate findings
  - [ ] Re-test

- [ ] **Encryption Validation**
  - [ ] Verify data is encrypted with correct algorithm
  - [ ] Verify X.509 v3 compliance
  - [ ] Verify RFC 5280 compliance
  - [ ] Test decryption with SRIJ key (if possible)

---

### 6.5 Disaster Recovery Testing

- [ ] **Backup Testing**
  - [ ] Perform full backup of Safe data
  - [ ] Perform restore test
  - [ ] Verify data integrity after restore
  - [ ] Measure restore time (must be <1 week per SRIJ)

- [ ] **Failover Testing**
  - [ ] Test Gateway failover (primary → DR site)
  - [ ] Test Captor failover
  - [ ] Test Safe failover
  - [ ] Measure failover time (target: <4 hours)
  - [ ] Test failback procedures

- [ ] **DR Drill**
  - [ ] Simulate full data center failure
  - [ ] Activate DR site
  - [ ] Verify all systems operational
  - [ ] Measure RTO (Recovery Time Objective)
  - [ ] Document lessons learned
  - [ ] Update DR procedures

---

### 6.6 Compliance Testing

- [ ] **SRIJ Data Model Compliance**
  - [ ] Verify all required data categories are captured
  - [ ] Verify data format matches SRIJ specifications
  - [ ] Verify folder structure is correct
  - [ ] Generate sample data export for SRIJ review

- [ ] **Data Retention Testing**
  - [ ] Verify 10-year retention capability
  - [ ] Test 24-month hot storage accessibility
  - [ ] Test cold storage retrieval (96-month data)

- [ ] **Audit Trail Testing**
  - [ ] Verify all SRIJ access is logged
  - [ ] Test log completeness and accuracy
  - [ ] Test log retention

---

## Phase 7: Independent Certification (Weeks 25-32)

### 7.1 Testing Laboratory Certification

- [ ] **Submit to Independent Lab**
  - [ ] Package all documentation
  - [ ] Provide lab with access to testing environment
  - [ ] Submit gaming platform for testing
  - [ ] Submit RNG for certification
  - [ ] Submit ERI components for review

- [ ] **Lab Testing Process**
  - [ ] Respond to lab questions and requests
  - [ ] Provide additional information as needed
  - [ ] Fix any issues identified
  - [ ] Re-submit for re-testing if needed

- [ ] **Obtain Certifications**
  - [ ] RNG certification report
  - [ ] Platform compliance certification
  - [ ] ERI compliance certification
  - [ ] Store all certificates securely

**Lab**: _______________
**Submitted**: _______________
**Certification Received**: _______________

---

### 7.2 Documentation Preparation

- [ ] **Compile Documentation Package for SRIJ**
  - [ ] System architecture diagrams
  - [ ] Network diagrams
  - [ ] Data flow diagrams
  - [ ] Server specifications
  - [ ] Software documentation
  - [ ] Security policies and procedures
  - [ ] Disaster recovery plan
  - [ ] Business continuity plan
  - [ ] Data Model implementation details
  - [ ] FTPS access instructions for SRIJ
  - [ ] Independent lab certifications
  - [ ] Company information and licenses

---

## Phase 8: SRIJ Homologation (Weeks 33-40)

### 8.1 License Application Submission

- [ ] **Submit Complete License Application**
  - [ ] Application forms (completed)
  - [ ] Corporate documentation
  - [ ] Background checks (key personnel)
  - [ ] Financial documentation
  - [ ] Financial guarantees (€500k + €100k)
  - [ ] Technical documentation
  - [ ] Independent lab certifications
  - [ ] .pt domain proof
  - [ ] Data center agreements
  - [ ] AML policies
  - [ ] Responsible gambling policies
  - [ ] Application fee payment

**Submitted to SRIJ**: _______________

---

### 8.2 SRIJ Technical Testing

- [ ] **Homologation Phase**
  - [ ] Provide SRIJ with access credentials (FTPS to Safe)
  - [ ] Provide connection details (IP addresses, ports)
  - [ ] SRIJ conducts technical tests per Homologation Manual
  - [ ] Respond to SRIJ technical queries
  - [ ] Fix any issues identified by SRIJ
  - [ ] Re-test as needed

- [ ] **SRIJ Testing Areas** (anticipated):
  - [ ] Gateway functionality (Portuguese IP detection, .pt domain enforcement)
  - [ ] Safe connectivity (FTPS access)
  - [ ] Data format compliance (SRIJ Data Model)
  - [ ] Encryption verification (X.509 v3)
  - [ ] Data retention (10-year capability)
  - [ ] Time synchronization (NTP)
  - [ ] Availability (uptime monitoring)
  - [ ] Captor processing speed

---

### 8.3 SRIJ Review and Approval

- [ ] **Await SRIJ Review**
  - [ ] SRIJ reviews all documentation
  - [ ] SRIJ may request additional information
  - [ ] SRIJ may conduct on-site inspection (if required)
  - [ ] Address all SRIJ concerns

- [ ] **Obtain SRIJ License**
  - [ ] Receive approval notification
  - [ ] Receive license certificate
  - [ ] Store license securely
  - [ ] Confirm effective date

**SRIJ License Approved**: _______________
**License Number**: _______________
**Effective Date**: _______________

---

## Phase 9: Pre-Production (Weeks 41-44)

### 9.1 Production Environment Setup

- [ ] **Production Deployment**
  - [ ] Deploy Gateway to production servers (primary + DR)
  - [ ] Deploy Captor to production servers
  - [ ] Deploy Safe to production servers
  - [ ] Configure production databases
  - [ ] Configure production DNS (.pt domain)
  - [ ] Configure production SSL certificates
  - [ ] Enable production monitoring
  - [ ] Enable production logging

---

### 9.2 SRIJ Production Integration

- [ ] **Connect to SRIJ Production Infrastructure**
  - [ ] Configure FTPS with production SRIJ credentials
  - [ ] Test connectivity to SRIJ production
  - [ ] Verify SRIJ can access Safe
  - [ ] Perform initial data transfer test
  - [ ] Confirm NTP synchronization with production NTP servers

---

### 9.3 Final Testing in Production

- [ ] **Production Readiness Testing**
  - [ ] End-to-end test in production environment
  - [ ] Verify DNS resolution (.pt domain)
  - [ ] Test player access from Portuguese IPs
  - [ ] Test event capture and storage
  - [ ] Test SRIJ data access
  - [ ] Verify all monitoring and alerting
  - [ ] Test failover in production (if low-risk window available)

---

### 9.4 Staff Training

- [ ] **Train Operations Team**
  - [ ] System administration procedures
  - [ ] Monitoring and alerting response
  - [ ] Incident response procedures
  - [ ] SRIJ reporting procedures
  - [ ] Escalation procedures
  - [ ] DR activation procedures

- [ ] **Train Customer Support** (if applicable)
  - [ ] Portuguese player requirements
  - [ ] .pt domain usage
  - [ ] SRIJ compliance awareness

---

### 9.5 Runbooks and Documentation

- [ ] **Create Operational Runbooks**
  - [ ] Gateway restart procedure
  - [ ] Captor restart procedure
  - [ ] Safe maintenance procedure
  - [ ] Failover procedure (to DR site)
  - [ ] Failback procedure (from DR site)
  - [ ] Backup and restore procedure
  - [ ] SRIJ monthly reporting procedure
  - [ ] Incident response procedure
  - [ ] Contact lists (internal, SRIJ, vendors)

---

## Phase 10: Pilot Launch (Weeks 45-48)

### 10.1 Soft Launch

- [ ] **Limited Player Access**
  - [ ] Define pilot user group (e.g., 100-1000 players)
  - [ ] Enable Portuguese player registration
  - [ ] Enable access via .pt domain
  - [ ] Monitor all systems intensively
  - [ ] Monitor player feedback
  - [ ] Monitor SRIJ data collection

---

### 10.2 Pilot Monitoring

- [ ] **Intensive Monitoring Period**
  - [ ] 24/7 on-call team during pilot
  - [ ] Daily system health reviews
  - [ ] Daily data quality checks
  - [ ] Monitor downtime (tracking toward 4-hour limit)
  - [ ] Monitor Safe storage growth
  - [ ] Monitor SRIJ FTPS access (ensure successful)

---

### 10.3 Issue Resolution

- [ ] **Address Any Issues**
  - [ ] Document all issues encountered
  - [ ] Prioritize and fix bugs
  - [ ] Optimize performance if needed
  - [ ] Update documentation

---

### 10.4 SRIJ Observation Period

- [ ] **SRIJ Monitoring**
  - [ ] Inform SRIJ of pilot launch
  - [ ] Provide SRIJ with access for observation
  - [ ] Respond to any SRIJ concerns
  - [ ] Provide SRIJ with pilot period report

---

## Phase 11: Full Production Launch (Week 49+)

### 11.1 Go-Live Preparation

- [ ] **Final Go-Live Checklist**
  - [ ] All pilot issues resolved
  - [ ] SRIJ approval for full launch (if required)
  - [ ] Monitoring confirmed operational
  - [ ] Backups confirmed operational
  - [ ] DR site confirmed operational
  - [ ] On-call rotation established
  - [ ] Marketing materials updated (.pt domain)

---

### 11.2 Production Launch

- [ ] **Full Launch**
  - [ ] Enable full player registration
  - [ ] Announce .pt domain to existing Portuguese players
  - [ ] Marketing campaign (if applicable)
  - [ ] Monitor launch closely (24-48 hours intensive)

**Production Launch Date**: _______________

---

### 11.3 Post-Launch

- [ ] **First Week**
  - [ ] Daily system health checks
  - [ ] Daily data quality verification
  - [ ] Monitor player adoption of .pt domain
  - [ ] Address any issues immediately

- [ ] **First Month**
  - [ ] Verify <4 hours total downtime
  - [ ] Generate first monthly SRIJ report
  - [ ] Submit report to SRIJ
  - [ ] Review and optimize as needed

---

## Phase 12: Ongoing Operations (Continuous)

### 12.1 Regular Operations

- [ ] **Daily**
  - [ ] Monitor system health
  - [ ] Review security alerts
  - [ ] Check SRIJ data transfers
  - [ ] Review error logs

- [ ] **Weekly**
  - [ ] Review capacity and performance
  - [ ] Review downtime tracking
  - [ ] Test backups
  - [ ] Security updates

- [ ] **Monthly**
  - [ ] Generate SRIJ report
  - [ ] Submit report to SRIJ
  - [ ] Review monthly downtime (ensure <4 hours)
  - [ ] Review storage capacity
  - [ ] Security patches
  - [ ] Review and update documentation

- [ ] **Quarterly**
  - [ ] Disaster recovery drill
  - [ ] Review and update DR procedures
  - [ ] Independent security assessment
  - [ ] Capacity planning review

- [ ] **Annually**
  - [ ] Independent lab re-certification (if required)
  - [ ] SRIJ license renewal
  - [ ] Full penetration testing
  - [ ] Review and update all policies and procedures
  - [ ] Hardware refresh planning
  - [ ] SSL certificate renewal (.pt domain)

---

### 12.2 SRIJ Compliance

- [ ] **Ongoing SRIJ Requirements**
  - [ ] Maintain permanent SRIJ access to Safe
  - [ ] Respond to SRIJ audits and inquiries
  - [ ] Submit monthly activity reports
  - [ ] Maintain 10-year data retention
  - [ ] Notify SRIJ of any significant changes (infrastructure, ownership, etc.)
  - [ ] Participate in SRIJ inspections

---

### 12.3 Continuous Improvement

- [ ] **Optimization**
  - [ ] Monitor performance trends
  - [ ] Optimize bottlenecks
  - [ ] Scale infrastructure as player base grows
  - [ ] Update security measures
  - [ ] Improve monitoring and alerting
  - [ ] Refine DR procedures based on drills

---

## Key Milestones Summary

| Milestone | Target Week | Status | Actual Date |
|-----------|-------------|---------|-------------|
| SRIJ initial contact | Week 1 | ☐ | ___________ |
| Data center selected | Week 8 | ☐ | ___________ |
| Hardware deployed | Week 14 | ☐ | ___________ |
| Application development complete | Week 16 | ☐ | ___________ |
| Internal testing complete | Week 24 | ☐ | ___________ |
| Independent lab certification | Week 32 | ☐ | ___________ |
| SRIJ license application submitted | Week 33 | ☐ | ___________ |
| SRIJ license approved | Week 40 | ☐ | ___________ |
| Pilot launch | Week 45 | ☐ | ___________ |
| Full production launch | Week 49 | ☐ | ___________ |

---

## Critical Success Factors

### Must-Haves for Success

1. ✅ **SRIJ Engagement**: Early and continuous communication with SRIJ
2. ✅ **Portuguese Data Center**: Tier III facility in Lisbon (recommended)
3. ✅ **Experienced Team**: DevOps and security specialists with gambling experience
4. ✅ **Financial Guarantees**: €600,000 total secured
5. ✅ **Independent Lab**: SRIJ-recognized testing laboratory engaged
6. ✅ **.pt Domain**: Registered and operational
7. ✅ **Compliance-First**: All development with SRIJ requirements in mind
8. ✅ **Testing**: Comprehensive testing before submission
9. ✅ **Documentation**: Complete and accurate technical documentation
10. ✅ **Time**: Realistic 11-12 month timeline

---

## Risk Management

### Common Risks and Mitigations

| Risk | Impact | Mitigation |
|------|--------|-----------|
| SRIJ requirement changes | High | Early engagement, continuous communication |
| Certification delays | High | Start early, choose experienced lab |
| Hardware delivery delays | Medium | Order early, have backup vendors |
| Data center issues | High | Choose Tier III provider, have DR site |
| Development delays | Medium | Experienced team, agile methodology |
| Network connectivity issues | High | Redundant connections, test early |
| SRIJ homologation failure | Critical | Thorough testing, engage consultants |
| Budget overruns | Medium | Detailed budgeting, 20% contingency |

---

## Budget Tracking

### Estimated Costs (Customize for Your Scale)

| Category | Estimated Cost | Actual Cost | Variance |
|----------|---------------|-------------|----------|
| Data center (Year 1) | €60,000 | €________ | €_______ |
| Hardware | €70,000 | €________ | €_______ |
| Software licenses | €15,000 | €________ | €_______ |
| Personnel | €200,000 | €________ | €_______ |
| Testing lab | €50,000 | €________ | €_______ |
| Financial guarantees | €600,000 (held) | €________ | €_______ |
| Legal/consulting | €30,000 | €________ | €_______ |
| Contingency (20%) | €85,000 | €________ | €_______ |
| **TOTAL (Year 1)** | **€1,110,000** | **€________** | **€_______** |

---

## Document Control

**Version**: 1.0
**Last Updated**: 2025-11-10
**Owner**: Project Manager
**Next Review**: Weekly during deployment

---

## Appendix: Contact Template

### Key Contacts

| Role | Name | Email | Phone | Notes |
|------|------|-------|-------|-------|
| SRIJ Liaison | _________ | ________ | ________ | Primary SRIJ contact |
| Project Manager | _________ | ________ | ________ | |
| DevOps Lead | _________ | ________ | ________ | |
| Security Lead | _________ | ________ | ________ | |
| Data Center (Primary) | _________ | ________ | ________ | 24/7 support |
| Data Center (DR) | _________ | ________ | ________ | |
| Testing Lab | _________ | ________ | ________ | |
| Legal Counsel | _________ | ________ | ________ | Portuguese gambling law |
| On-Call Rotation | _________ | ________ | ________ | |

---

**Good luck with your SRIJ ERI deployment! This is a complex but achievable project with proper planning and execution.**

For questions or clarifications, refer to:
- SRIJ_ERI_REQUIREMENTS.md
- TECHNICAL_ARCHITECTURE.md
- HOSTING_REQUIREMENTS.md
- Official SRIJ documentation at https://www.srij.turismodeportugal.pt
