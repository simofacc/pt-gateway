# SRIJ ERI Gateway Requirements - Complete Documentation

## Overview

This document outlines the complete requirements for building a compliant Entry and Registry Infrastructure (ERI) gateway for online gambling operators in Portugal, as mandated by SRIJ (Serviço de Regulação e Inspeção de Jogos).

## Regulatory Framework

**Primary Regulation**: Regulamento 903-B/2015, de 23 de Dezembro
**Legal Framework**: Decreto-Lei 66/2015 (RJO - Legal Regime for Online Games and Betting)
**Regulatory Authority**: SRIJ - Turismo de Portugal

---

## 1. Entry and Registry Infrastructure (IER/ERI) Components

The ERI consists of two mandatory components that must be maintained in Portuguese territory:

### 1.1 Gateway
A dedicated gateway through which all player access must be routed.

### 1.2 Safe (Cofre)
A secure data storage infrastructure for gambling and betting data retention.

---

## 2. Gateway Requirements

### 2.1 Access Routing
- **MANDATORY**: All player access from Portuguese territory (Portuguese IP addresses) OR using Portuguese-registered player accounts MUST be redirected through the Gateway
- Applies to: Web access, mobile access, and all other access methods

### 2.2 Domain Requirements
- **MANDATORY**: Access to the gaming platform via the Gateway can ONLY be performed through a top-level internet domain ending in ".pt"
- Example: `operatorname.pt`
- International domains (`.com`, `.net`, etc.) cannot be used for Portuguese players

### 2.3 Gateway Location
- Must be installed and maintained within Portuguese territory
- The gateway serves as the entry point for all Portuguese gaming traffic

### 2.4 Data Accessibility
- Gateway data must be maintained in a usable, auditable format
- SRIJ must have access to gateway data for audit purposes

### 2.5 Gateway Technical Specifications
- Must handle all player traffic from Portuguese territory
- Must integrate with the Safe infrastructure for data logging
- Must support redirection from international platforms to .pt domain
- Must maintain connection to SRIJ control infrastructure

---

## 3. Safe (Cofre) Infrastructure Requirements

### 3.1 Location
- **MANDATORY**: Must be physically located in Portuguese territory
- Must be hosted in a data center within Portugal

### 3.2 Data Storage Requirements

#### 3.2.1 Data Retention Period
- **Total retention**: Minimum 120 months (10 years)
- **Immediately accessible**: Latest 24 months must be online and immediately accessible
- **Backup storage**: Remaining 96 months can be stored on digital backup media

#### 3.2.2 Data Categories
The Safe must store all gambling and betting data according to SRIJ-defined categories, including:
- Player transactions
- Game outcomes
- Betting records
- Player account activities
- All gaming-related events

### 3.3 Folder Structure
- Must follow the structure and frequency specified by SRIJ's Data Model
- Data organization must comply with SRIJ specifications

### 3.4 Data Security

#### 3.4.1 Encryption Requirements
- Each data category must be:
  - **Signed**: Digital signature for integrity
  - **Compressed**: For efficient storage
  - **Encrypted**: Using SRIJ-provided Multicert Public Key
- Encryption standards: ITU X.509 version 3 and RFC 5280

#### 3.4.2 Data Formats
- Must use formats specified in SRIJ's Data Model
- Data must be in a usable format for audit processes

### 3.5 SRIJ Access Requirements
- **MANDATORY**: Permanent access for SRIJ to the Safe
- Purpose: Data consultation and collection as part of control and inspection
- Access method: FTPS protocol

### 3.6 Technical Infrastructure Specifications

#### 3.6.1 Operating System
- **Tested and compatible**: Oracle Linux, Red Hat
- Must run on Linux-based system

#### 3.6.2 Network Connection
- **Minimum bandwidth**: 20 Mbps dedicated broadband connection
- Connection must be dedicated to SRIJ control infrastructure
- Must support FTPS service

#### 3.6.3 FTPS Service
- Must be configured at the operating system level
- Used for data transfer to SRIJ
- Secure data transfer between Captor and Safe (FTPS, HTTPS, or equivalent)

---

## 4. Time Synchronization Requirements

### 4.1 NTP Synchronization
- **MANDATORY**: All platform and IER elements must synchronize with a single reliable time reference
- **Time Standard**: Portugal Continental legal time
- **NTP Servers**: Must use NTP servers managed by the Lisbon Astronomical Observatory
- Ensures consistent timestamping across all gaming events

---

## 5. Captor Component

The Captor is part of the ERI that captures gaming events:

### 5.1 Performance Requirements
- Must process information at the platform's minimum speed
- Cannot create bottlenecks in the gaming platform

### 5.2 Data Transfer
- Must transfer data to the Safe using secure protocols
- Supported protocols: FTPS, HTTPS, or equivalent

---

## 6. Availability and Reliability Requirements

### 6.1 Maximum Downtime
- **Combined Captor/Safe downtime**: Cannot exceed 4 hours per month
- Critical infrastructure requiring high availability

### 6.2 Disaster Recovery
- Data loss recovery must occur within **one week**
- Must have disaster recovery procedures in place

### 6.3 Business Continuity
- Business continuity plans required
- Must enable operations resumption within **one month** of a disaster

### 6.4 Critical Infrastructure Classification
- The IER is classified as a **critical component**
- Security levels must match the gaming platform itself
- Must implement all standard critical infrastructure protections

---

## 7. Hosting Location Requirements

### 7.1 Physical Location
- **MANDATORY**: Both Gateway and Safe infrastructure must be physically located in **Portuguese territory**
- This is a legal requirement under Portuguese gambling law

### 7.2 Data Center Selection Criteria
When selecting a data center in Portugal, consider:

1. **Geographic Location**: Must be within Portugal's borders
2. **Connectivity**: Minimum 20 Mbps dedicated connection to SRIJ
3. **Security**: ISO 27001 compliance recommended
4. **Availability**: High uptime SLAs (99.9%+ recommended)
5. **Compliance**: Must support regulatory requirements
6. **Disaster Recovery**: Redundancy and backup capabilities

### 7.3 Recommended Portuguese Data Centers
Consider the following types of facilities in Portugal:
- Tier III or Tier IV data centers
- Facilities in Lisbon, Porto, or other major Portuguese cities
- Providers with experience in regulated industries
- Facilities with direct connectivity to SRIJ infrastructure

**NOTE**: You should contact SRIJ directly to confirm approved or recommended data center providers.

---

## 8. Network and Firewall Requirements

### 8.1 Network Architecture
- Dedicated network connection to SRIJ control infrastructure
- Minimum 20 Mbps bandwidth allocation
- Must support FTPS traffic to SRIJ

### 8.2 Firewall Configuration
While specific firewall rules are not publicly documented, the following are recommended:

#### 8.2.1 Inbound Rules
- Allow FTPS connections from SRIJ IP addresses (obtain from SRIJ)
- Allow NTP from Lisbon Astronomical Observatory NTP servers
- Allow player traffic to Gateway from Portuguese IP ranges
- Block all other unauthorized access

#### 8.2.2 Outbound Rules
- Allow FTPS connections to SRIJ infrastructure
- Allow NTP synchronization traffic
- Allow DNS resolution
- Allow routing to main gaming platform (if separate)

#### 8.2.3 Security Best Practices
- Implement DDoS protection
- Use intrusion detection/prevention systems (IDS/IPS)
- Enable comprehensive logging
- Implement rate limiting
- Use geo-blocking for non-Portuguese traffic (except authorized connections)

### 8.3 Routing Requirements
- Route all Portuguese player traffic through Gateway
- Implement IP geolocation to identify Portuguese players
- Redirect Portuguese players to .pt domain
- Maintain routing tables for SRIJ connectivity

---

## 9. Security Requirements

### 9.1 Information Security Management
- Align with ISO 27001 practices
- SRIJ operates its own information security management system
- Operators should implement comparable security standards

### 9.2 Data Protection
- **GDPR Compliance**: Mandatory
- Technical and organizational measures to protect player privacy
- Secure storage and transmission of all gaming data

### 9.3 Cybersecurity
- Gaming platform must meet SRIJ cybersecurity standards
- Regular security audits required
- Vulnerability management program

---

## 10. Certification and Testing Requirements

### 10.1 Independent Testing Laboratory
- Must obtain certification from a testing laboratory recognized by SRIJ
- Certification must cover the entire platform available in Portugal
- Must verify compliance with all Portuguese laws and regulations

### 10.2 RNG Certification
- Random Number Generators must be certified
- Required for all chance-based games

### 10.3 SRIJ Technical Testing
- After independent lab certification, SRIJ's technical team will conduct their own tests
- Tests verify integration with SRIJ control infrastructure
- Connection between Gateway and SRIJ infrastructure must be validated

### 10.4 Homologation Process
- Follow procedures outlined in SRIJ's Homologation Manual
- Includes connection specifications and testing requirements
- Must complete successfully before going live

---

## 11. Financial Guarantees

Operators must provide:

1. **Player Liability Guarantee**: €500,000 per license
   - Format: Guarantee, insurance, or bank deposit
   - Purpose: Collateral for performance of all legal obligations

2. **Tax Payment Guarantee**: €100,000
   - Purpose: Collateral for payment of IEJO (special online gambling tax)

---

## 12. Ongoing Compliance Requirements

### 12.1 Reporting
- Monthly activity reporting to SRIJ
- Must provide data in SRIJ-specified formats

### 12.2 Audits
- SRIJ may conduct remote audits at any time
- Must maintain permanent access for SRIJ to Safe infrastructure
- Must cooperate with inspection activities

### 12.3 Periodic Reviews
- Independent third-party laboratory audits on a periodic basis
- Software integrity verification
- System compliance re-certification as required

---

## 13. Technical Implementation Checklist

### Phase 1: Infrastructure Setup
- [ ] Select and contract with a data center in Portuguese territory
- [ ] Provision servers for Gateway component
- [ ] Provision servers for Safe component with sufficient storage for 10 years
- [ ] Set up Linux environment (Oracle Linux or Red Hat)
- [ ] Configure minimum 20 Mbps dedicated network connection

### Phase 2: Gateway Implementation
- [ ] Register .pt domain name
- [ ] Configure Gateway to intercept Portuguese player traffic
- [ ] Implement IP geolocation for Portuguese IP detection
- [ ] Set up redirection for Portuguese players to .pt domain
- [ ] Configure Gateway to route traffic to main gaming platform
- [ ] Implement logging and monitoring

### Phase 3: Safe Implementation
- [ ] Set up SRIJ-defined folder structure
- [ ] Implement data encryption using Multicert Public Key (ITU X.509 v3, RFC 5280)
- [ ] Configure data signing mechanism
- [ ] Implement data compression
- [ ] Set up 24-month immediate access storage
- [ ] Set up 96-month backup storage system
- [ ] Configure FTPS service for SRIJ access

### Phase 4: Captor Implementation
- [ ] Develop/configure Captor to capture all gaming events
- [ ] Implement data categorization per SRIJ Data Model
- [ ] Configure secure data transfer to Safe (FTPS/HTTPS)
- [ ] Optimize for platform-speed processing

### Phase 5: Network and Security
- [ ] Configure NTP synchronization with Lisbon Astronomical Observatory
- [ ] Set up firewall rules for SRIJ access
- [ ] Implement security controls (IDS/IPS, DDoS protection)
- [ ] Configure VPN or secure tunnel to SRIJ (if required)
- [ ] Set up monitoring and alerting

### Phase 6: SRIJ Integration
- [ ] Obtain SRIJ infrastructure connection details
- [ ] Configure FTPS connection to SRIJ
- [ ] Provide SRIJ with permanent access credentials to Safe
- [ ] Test data transfer to SRIJ
- [ ] Verify NTP synchronization

### Phase 7: Disaster Recovery
- [ ] Implement backup systems
- [ ] Create disaster recovery procedures
- [ ] Develop business continuity plan
- [ ] Test recovery within one-week timeframe
- [ ] Document operations resumption plan (one-month target)

### Phase 8: Certification
- [ ] Engage SRIJ-recognized independent testing laboratory
- [ ] Obtain platform certification
- [ ] Obtain RNG certification
- [ ] Submit to SRIJ technical testing
- [ ] Complete homologation process
- [ ] Address any issues identified during testing

### Phase 9: Financial and Legal
- [ ] Arrange €500,000 player liability guarantee
- [ ] Arrange €100,000 tax payment guarantee
- [ ] Submit license application with all documentation
- [ ] Obtain SRIJ license

### Phase 10: Go-Live
- [ ] Configure .pt domain DNS
- [ ] Enable Gateway for Portuguese traffic
- [ ] Begin data collection in Safe
- [ ] Start monthly reporting to SRIJ
- [ ] Monitor compliance continuously

---

## 14. Contact and Resources

### SRIJ Contact
- **Website**: https://www.srij.turismodeportugal.pt
- **Official Documentation**: Available on SRIJ website (primarily in Portuguese)

### Key Documents to Obtain from SRIJ
1. Homologation Manual (Manual de Homologação)
2. Data Model Specifications
3. SRIJ Infrastructure Connection Details
4. List of Recognized Testing Laboratories
5. Technical Regulations (Regulamento Técnico)
6. Multicert Public Key for encryption

### Important Notes
- Most SRIJ documentation is in Portuguese
- Direct contact with SRIJ is recommended for specific technical details
- Requirements may be updated; always verify with latest SRIJ regulations
- Consider hiring Portuguese legal counsel familiar with gambling regulations
- Consider engaging consultants with SRIJ implementation experience

---

## 15. Summary of Critical Requirements

### Must Be in Portugal
✅ Gateway infrastructure
✅ Safe infrastructure
✅ Data center hosting

### Must Use .pt Domain
✅ All Portuguese player access

### Must Provide SRIJ Access
✅ Permanent FTPS access to Safe
✅ Remote audit capabilities

### Must Retain Data
✅ 10 years total (120 months)
✅ 24 months immediately accessible

### Must Be Certified
✅ Independent laboratory certification
✅ SRIJ technical testing
✅ RNG certification

### Must Synchronize Time
✅ NTP with Lisbon Astronomical Observatory

### Maximum Downtime
✅ 4 hours per month (combined Captor/Safe)

### Data Security
✅ Sign, compress, encrypt all data
✅ ITU X.509 v3 / RFC 5280 standards

---

## Document Version
**Version**: 1.0
**Date**: 2025-11-10
**Based on**: Regulamento 903-B/2015 and SRIJ public information
**Status**: Comprehensive requirements documentation

---

**DISCLAIMER**: This document is based on publicly available information about SRIJ requirements. Always verify all requirements directly with SRIJ and obtain official technical documentation before implementation. Regulations may change, and specific implementation details may require direct consultation with SRIJ.
