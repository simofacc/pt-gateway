# SRIJ ERI Gateway - Hosting Location Requirements

## Executive Summary

**MANDATORY REQUIREMENT**: All Entry and Registry Infrastructure (ERI) components, including the Gateway and Safe, **MUST be physically hosted in Portuguese territory** as mandated by SRIJ regulations.

This document provides comprehensive guidance on hosting requirements and data center selection in Portugal.

---

## 1. Legal Hosting Requirements

### 1.1 Regulatory Mandate

**Source**: Regulamento 903-B/2015, Decreto-Lei 66/2015 (RJO)

**Requirement**:
> "Part of [the technical gaming system] must be maintained in Portugal by the respective online gambling operators, the so-called 'entry and registry infrastructure', which includes the gateway and the safe."

**Components That MUST Be in Portugal**:
1. ✅ **Gateway Infrastructure** - All servers handling Portuguese player traffic
2. ✅ **Safe Infrastructure** - All data storage and FTPS servers
3. ✅ **Captor Infrastructure** - Event capture and processing (part of ERI)

**Components That CAN Be Outside Portugal**:
1. Main gaming platform servers (game logic, RNG)
2. Primary player database (if separated from Safe)
3. Payment processing infrastructure
4. Marketing and CRM systems

**IMPORTANT**: Even if the main gaming platform is hosted internationally, the Gateway, Captor, and Safe **must** be in Portugal.

---

## 2. Geographic Requirements

### 2.1 Portugal Territory Definition

**Acceptable Locations**:
- Continental Portugal (Mainland)
- Autonomous Region of Madeira
- Autonomous Region of Azores

**Recommended Regions**:
1. **Lisbon Metropolitan Area** (Primary recommendation)
   - Proximity to SRIJ headquarters (Turismo de Portugal)
   - Best network infrastructure
   - Most data center options
   - Lowest latency to SRIJ control infrastructure

2. **Porto Metropolitan Area** (Secondary recommendation)
   - Second largest city
   - Good infrastructure
   - Disaster recovery site option
   - Geographic diversity from Lisbon

3. **Other Acceptable Locations**:
   - Coimbra
   - Braga
   - Faro
   - Madeira (for specific requirements)
   - Azores (for specific requirements)

### 2.2 Why Portugal?

The Portuguese government requires local hosting to ensure:
- **Regulatory Control**: SRIJ can physically access infrastructure if needed
- **Data Sovereignty**: Portuguese player data remains under Portuguese jurisdiction
- **Law Enforcement**: Compliance with Portuguese legal system
- **Tax Compliance**: Easier enforcement of Portuguese gambling taxes
- **Player Protection**: Portuguese consumer protection laws apply
- **Network Performance**: Low latency for Portuguese players

---

## 3. Data Center Selection Criteria

### 3.1 Essential Requirements

#### 3.1.1 Location
- [ ] **MANDATORY**: Physical location within Portuguese territory
- [ ] Verifiable physical address in Portugal
- [ ] Legal registration as Portuguese entity or foreign entity operating in Portugal
- [ ] Facility can be inspected by SRIJ if required

#### 3.1.2 Certification and Compliance
- [ ] ISO 27001 (Information Security Management) - **Highly Recommended**
- [ ] ISO 9001 (Quality Management)
- [ ] Tier II or higher classification (**Tier III recommended**)
- [ ] GDPR compliant
- [ ] SOC 2 Type II (if available)
- [ ] PCI DSS (if handling payment data)
- [ ] Portuguese business licenses and permits

#### 3.1.3 Network Connectivity
- [ ] Minimum 1 Gbps connectivity
- [ ] Multiple upstream providers (redundancy)
- [ ] Direct peering with major Portuguese ISPs:
  - MEO (Altice Portugal)
  - NOS
  - Vodafone Portugal
  - NOWO
- [ ] Low latency to SRIJ infrastructure (<10ms recommended)
- [ ] Ability to provision **dedicated 20+ Mbps connection to SRIJ**
- [ ] IPv4 and IPv6 support
- [ ] DDoS protection available

#### 3.1.4 Power and Environmental
- [ ] Redundant power feeds (N+1 minimum)
- [ ] UPS systems with sufficient battery runtime (>30 minutes)
- [ ] Backup diesel generators
- [ ] Automatic failover between power sources
- [ ] N+1 cooling redundancy
- [ ] Environmental monitoring (temperature, humidity)
- [ ] Fire suppression systems

#### 3.1.5 Physical Security
- [ ] 24/7/365 on-site security personnel
- [ ] Biometric access control systems
- [ ] Multi-factor authentication for physical access
- [ ] Mantrap/airlock entry systems
- [ ] CCTV surveillance (24/7 recording)
- [ ] Visitor escort policy
- [ ] Rack-level locking
- [ ] Security audit logs

#### 3.1.6 Support and SLA
- [ ] 24/7/365 technical support
- [ ] Portuguese-speaking staff
- [ ] On-site "remote hands" service
- [ ] Guaranteed uptime SLA: **99.95% or higher** (recommended 99.99%)
- [ ] Response time SLA for critical issues (<15 minutes)
- [ ] Dedicated account manager
- [ ] Regular maintenance windows with advance notice

---

### 3.2 Desirable Features

#### 3.2.1 Infrastructure
- [ ] Multiple data halls (for internal redundancy)
- [ ] Cage or private suite options (for security)
- [ ] Meet-me rooms for cross-connects
- [ ] Direct cloud connectivity (AWS Direct Connect, Azure ExpressRoute)
- [ ] Carrier-neutral facility

#### 3.2.2 Disaster Recovery
- [ ] Partner facility in different Portuguese city (for DR)
- [ ] Geographic diversity options
- [ ] Data replication services
- [ ] Backup storage services

#### 3.2.3 Compliance Support
- [ ] Experience with regulated industries (financial, healthcare, gambling)
- [ ] Audit support services
- [ ] Compliance documentation and reports
- [ ] Regular third-party audits

#### 3.2.4 Operational
- [ ] Remote console access (IPMI, iLO, iDRAC)
- [ ] Customer portal for monitoring and management
- [ ] Billing transparency
- [ ] Flexible contract terms
- [ ] Scalability (easy to add racks/capacity)

---

## 4. Portuguese Data Center Providers

### 4.1 Major Providers in Portugal

#### 4.1.1 **Equinix** (International, Portugal Presence)
- **Locations**: Lisbon (LS1, LS2 data centers)
- **Tier**: Tier III
- **Certifications**: ISO 27001, ISO 9001, PCI DSS, SOC 2
- **Website**: equinix.com/locations/europe-colocation/portugal-colocation
- **Notes**:
  - Global provider with strong presence in Lisbon
  - Excellent connectivity and carrier options
  - Premium pricing
  - Strong compliance and security posture
  - Good for enterprises

**Recommendation**: ★★★★★ (Highly Recommended)

---

#### 4.1.2 **Interxion** (Digital Realty - Portugal)
- **Locations**: Lisbon
- **Tier**: Tier III
- **Certifications**: ISO 27001, ISO 9001
- **Notes**:
  - Part of Digital Realty (since 2020)
  - Strong European network
  - Good connectivity options
  - Reliable infrastructure

**Recommendation**: ★★★★★ (Highly Recommended)

---

#### 4.1.3 **Claranet Portugal**
- **Locations**: Lisbon
- **Type**: Managed services and colocation
- **Certifications**: ISO 27001
- **Website**: claranet.pt
- **Notes**:
  - Portuguese and European presence
  - Managed hosting services available
  - Good for turnkey solutions
  - Portuguese-speaking support

**Recommendation**: ★★★★☆ (Recommended)

---

#### 4.1.4 **PTIN** (Portugal Telecom Infrastructure)
- **Type**: Telecommunications carrier with data center services
- **Locations**: Multiple locations across Portugal
- **Notes**:
  - National telecommunications provider
  - Strong Portuguese network presence
  - May have direct connectivity advantages for SRIJ
  - Government and enterprise focus

**Recommendation**: ★★★★☆ (Recommended - investigate SRIJ connectivity)

---

#### 4.1.5 **Vodafone Portugal Data Centers**
- **Type**: Telecommunications carrier data centers
- **Locations**: Lisbon, Porto
- **Notes**:
  - Major Portuguese telecom operator
  - Enterprise-grade infrastructure
  - Good network connectivity
  - Portuguese-speaking support

**Recommendation**: ★★★☆☆ (Worth Evaluating)

---

#### 4.1.6 **NOS Data Centers**
- **Type**: Telecommunications carrier data centers
- **Locations**: Portugal
- **Notes**:
  - Portuguese telecommunications operator
  - Cable and fiber network
  - Enterprise services

**Recommendation**: ★★★☆☆ (Worth Evaluating)

---

#### 4.1.7 **Lusavouga**
- **Locations**: Lisbon
- **Type**: Portuguese data center provider
- **Notes**:
  - Local Portuguese provider
  - May have competitive pricing
  - Smaller scale than international providers

**Recommendation**: ★★★☆☆ (Evaluate for smaller deployments)

---

### 4.2 Selection Matrix

| Provider | Location | Tier | ISO 27001 | Connectivity | Cost | Best For |
|----------|----------|------|-----------|--------------|------|----------|
| Equinix | Lisbon | III | ✅ | ★★★★★ | €€€€ | Enterprise, High Compliance |
| Interxion | Lisbon | III | ✅ | ★★★★★ | €€€€ | Enterprise |
| Claranet | Lisbon | II/III | ✅ | ★★★★☆ | €€€ | Managed Services |
| PTIN | Multiple | II/III | ✅ | ★★★★☆ | €€€ | Local Connectivity |
| Vodafone PT | Lisbon/Porto | II/III | ✅ | ★★★★☆ | €€€ | Telecom Integration |
| NOS | Multiple | II | ? | ★★★☆☆ | €€ | Budget-Conscious |
| Lusavouga | Lisbon | II | ? | ★★★☆☆ | €€ | Small Deployments |

**Cost Legend**:
- €€€€ = Premium (€3,000+/month per rack)
- €€€ = Mid-range (€1,500-€3,000/month per rack)
- €€ = Budget (€800-€1,500/month per rack)

---

## 5. SRIJ Connectivity Requirements

### 5.1 Network Requirements to SRIJ

**Mandatory Specifications**:
- **Minimum Bandwidth**: 20 Mbps dedicated to SRIJ control infrastructure
- **Recommended Bandwidth**: 50-100 Mbps (for headroom and future growth)
- **Protocol**: FTPS (FTP over TLS)
- **Ports**: 990 (FTPS), 21 (FTP control)
- **Latency**: As low as possible (<10ms recommended)
- **Availability**: 99.95%+ uptime

### 5.2 Questions to Ask Data Center Provider

Before selecting a data center, ask:

1. **SRIJ Connectivity**:
   - "Do you have any existing customers connecting to SRIJ infrastructure?"
   - "What is the network latency to SRIJ control infrastructure?" (Provide SRIJ IPs)
   - "Can you provision a dedicated connection or QoS for SRIJ traffic?"
   - "What routing path will our traffic take to reach SRIJ?"

2. **Network**:
   - "Which Tier 1 carriers do you have connectivity to?"
   - "Do you peer with major Portuguese ISPs?"
   - "What is your peering policy?"
   - "Can we bring our own IP addresses (BYOIP)?"

3. **Compliance**:
   - "Do you have experience hosting gambling/gaming infrastructure?"
   - "Can you provide audit support for regulatory compliance?"
   - "What compliance certifications do you hold?"
   - "Can SRIJ inspectors physically visit the facility if needed?"

4. **Disaster Recovery**:
   - "Do you have a partner facility in another Portuguese city?"
   - "Can you support geographic redundancy?"
   - "What are your data replication options?"

5. **Support**:
   - "What is your guaranteed response time for critical issues?"
   - "Do you have Portuguese-speaking technical staff 24/7?"
   - "What remote hands services do you offer?"

---

## 6. Contact SRIJ for Hosting Guidance

### 6.1 Questions to Ask SRIJ

**CRITICAL**: Before finalizing data center selection, contact SRIJ to ask:

1. **Preferred/Approved Data Centers**:
   - "Does SRIJ have a list of approved or recommended data center providers in Portugal?"
   - "Are there any data centers where SRIJ already has connectivity established?"
   - "Are there data centers we should avoid?"

2. **Network Connectivity**:
   - "What are the SRIJ control infrastructure IP addresses we need to connect to?"
   - "What is the required bandwidth for the FTPS connection?"
   - "Are there specific network requirements or QoS needed?"
   - "Do you provide VPN or direct connect options?"

3. **Physical Location**:
   - "Are all regions of Portugal acceptable (Mainland, Madeira, Azores)?"
   - "Is Lisbon proximity preferred for technical or inspection reasons?"

4. **Technical Specifications**:
   - "Do you have specific data center certification requirements?"
   - "Are there security requirements for the physical facility?"
   - "Do you require specific audit rights to the data center?"

**SRIJ Contact Information**:
- **Website**: https://www.srij.turismodeportugal.pt
- **Email**: Check SRIJ website for current contact email
- **Phone**: Check SRIJ website for current contact phone
- **Address**: Turismo de Portugal, Rua Ivone Silva, Lote 6, 1050-124 Lisboa, Portugal

---

## 7. Dual Data Center Strategy (Recommended)

### 7.1 Primary + Disaster Recovery Configuration

For high availability and compliance with the 4-hour monthly downtime limit:

**Primary Site**: Lisbon
- Full production infrastructure
- Gateway, Captor, Safe (hot storage)
- FTPS server for SRIJ
- All production traffic

**DR Site**: Porto (or secondary Lisbon facility)
- Standby infrastructure
- Real-time replication of Safe data
- Automated failover capability
- SRIJ connectivity maintained

**Benefits**:
- Geographic redundancy within Portugal
- Meet <4 hour downtime requirement
- Disaster resilience
- Maintenance flexibility

**Considerations**:
- Higher cost (2× infrastructure)
- Data replication bandwidth requirements
- Complexity in management
- Both sites must have SRIJ connectivity

---

## 8. Cloud vs. Colocation vs. Dedicated

### 8.1 Hosting Options in Portugal

#### 8.1.1 **Colocation** (Recommended)
**Description**: Rent rack space and bring your own servers

**Pros**:
- Full hardware control
- Customizable configuration
- No hypervisor overhead
- Good for compliance (physical control)
- Predictable performance

**Cons**:
- Higher upfront hardware costs
- Longer deployment time
- Hardware maintenance responsibility

**Best For**: SRIJ ERI deployment (recommended)

**Estimated Cost**: €2,000-€5,000/month + hardware

---

#### 8.1.2 **Dedicated Servers** (Alternative)
**Description**: Rent pre-configured physical servers

**Pros**:
- Faster deployment than colocation
- No hardware purchase
- Provider handles hardware issues
- Predictable monthly cost

**Cons**:
- Less hardware customization
- May not meet all specifications
- Provider lock-in

**Best For**: Quick deployment, smaller operators

**Estimated Cost**: €500-€2,000/month per server

---

#### 8.1.3 **Cloud (IaaS)** (Not Recommended for Safe)
**Description**: Virtual machines in Portuguese cloud regions

**Available in Portugal**:
- AWS Europe (Lisbon) - Not available as of 2025
- Azure (no Portugal region currently)
- Google Cloud (no Portugal region currently)
- Local Portuguese cloud providers (limited)

**Pros**:
- Fast provisioning
- Easy scaling
- Pay-as-you-go

**Cons**:
- **Limited availability in Portugal**
- Compliance concerns (multi-tenancy)
- Less control over physical security
- May not meet SRIJ requirements for critical infrastructure
- Storage performance variability

**Best For**: Development/testing environments only

**SRIJ Compliance**: ⚠️ Verify with SRIJ before using cloud infrastructure for production

---

### 8.2 Recommended Approach

**For Gateway & Captor**: Dedicated servers or Colocation
**For Safe**: Colocation (for maximum control and compliance)

**Reasoning**:
- Safe stores 10 years of sensitive data → maximum security control needed
- SRIJ may require physical audit access → colocation provides this
- Performance guarantees → dedicated hardware ensures compliance
- Regulatory clarity → physical servers in Portugal are unambiguous

---

## 9. Network Design for Portuguese Hosting

### 9.1 IP Addressing

**Option 1: Provider-Assigned IPs**
- Use data center's IP allocation
- Faster setup
- Provider handles BGP

**Option 2: Bring Your Own IP (BYOIP)**
- Use your own AS number and IP block
- More control
- Portability between providers
- Requires BGP configuration

**Recommendation**: Use provider IPs initially, plan BYOIP for long-term

---

### 9.2 Network Topology

```
Internet (Portuguese ISPs)
        │
        ├─ MEO (Altice Portugal)
        ├─ NOS
        ├─ Vodafone Portugal
        └─ NOWO
        │
        ▼
    Data Center
        │
    BGP Router
        │
    Firewall / DDoS Protection
        │
    ┌────┴────┐
    │         │
  Gateway   SRIJ
   DMZ      Subnet
            (dedicated)
```

---

### 9.3 Portuguese ISP Considerations

Major ISPs to consider for player traffic optimization:
1. **MEO** (Altice Portugal) - Largest ISP
2. **NOS** - Second largest, cable/fiber
3. **Vodafone Portugal** - Mobile and fixed
4. **NOWO** - Growing fiber provider

Ensure your data center has good peering with these ISPs for low-latency player access.

---

## 10. Hosting Cost Estimates (Portugal)

### 10.1 Colocation Costs

**Lisbon (Equinix/Interxion - Premium)**:
```
- Half rack: €2,500 - €3,500/month
- Full rack: €4,000 - €6,000/month
- Power: €200-€300 per kW/month
- Bandwidth (1 Gbps): €500 - €1,000/month
- Cross-connects: €100-€200 each/month
- Remote hands: €150-€250 per hour
```

**Lisbon (Local Providers - Mid-Range)**:
```
- Half rack: €1,500 - €2,500/month
- Full rack: €2,500 - €4,000/month
- Power: Included or €150-€250 per kW/month
- Bandwidth (1 Gbps): €300-€700/month
- Cross-connects: €50-€150 each/month
```

### 10.2 Dedicated Server Costs

```
Gateway Server (4 cores, 16GB RAM, 500GB SSD):
€300 - €600/month

Safe Server (8 cores, 32GB RAM, 10TB storage):
€600 - €1,200/month

Network (1 Gbps unmetered):
€300 - €600/month
```

### 10.3 Total Hosting Budget (Annual)

**Small Deployment** (Dedicated Servers):
- 2× Gateway servers: €600/mo × 12 = €7,200
- 2× Captor servers: €600/mo × 12 = €7,200
- 2× Safe servers: €1,200/mo × 12 = €14,400
- Bandwidth: €600/mo × 12 = €7,200
- **Total: ~€36,000/year**

**Medium Deployment** (Colocation):
- 1× Full rack: €4,000/mo × 12 = €48,000
- Hardware (one-time): €70,000
- Bandwidth: €800/mo × 12 = €9,600
- **Total Year 1: ~€127,600**
- **Total Year 2+: ~€57,600/year**

**Large Deployment** (Multi-DC Colocation):
- Primary site: €60,000/year
- DR site: €40,000/year
- Hardware: €150,000 (one-time)
- **Total Year 1: ~€250,000**
- **Total Year 2+: ~€100,000/year**

---

## 11. RFP Template for Data Center Providers

Use this template when requesting quotes from Portuguese data centers:

```
Subject: RFP for Data Center Services - SRIJ-Regulated Gambling Infrastructure

Dear [Provider],

We are seeking colocation/dedicated server services in Portugal for our
Entry and Registry Infrastructure (ERI) to comply with SRIJ gambling regulations.

REQUIREMENTS:

1. Location: Must be physically located in Portuguese territory
2. Compliance: ISO 27001 certified, Tier II or higher
3. Connectivity:
   - Minimum 1 Gbps internet connectivity
   - Dedicated 20+ Mbps connection to SRIJ infrastructure
   - Low latency to SRIJ control infrastructure
4. Infrastructure:
   - [X] rack units / [X] racks
   - [X] kW power allocation
   - Redundant power and cooling
5. Support: 24/7/365 Portuguese-speaking technical support
6. SLA: 99.95%+ uptime guarantee
7. Security: 24/7 security, biometric access, CCTV

SPECIFIC QUESTIONS:

1. Do you have experience hosting gambling/gaming infrastructure?
2. Do you have existing customers connecting to SRIJ infrastructure?
3. What is the network latency to [SRIJ IP addresses - obtain from SRIJ]?
4. Can SRIJ regulatory inspectors visit the facility if needed?
5. What compliance certifications do you hold?
6. Do you offer disaster recovery options in another Portuguese location?

REQUESTED INFORMATION:

- Detailed pricing (setup, monthly recurring)
- SLA terms
- Contract terms and minimum commitment
- Data center certifications
- Network topology and carrier options
- Disaster recovery options
- Compliance support

Please provide a quote and relevant documentation by [DATE].

Regards,
[Your Company]
```

---

## 12. Final Hosting Recommendations

### 12.1 Recommended Configuration

**For Most Operators**:

**Primary Production Site**:
- **Location**: Lisbon (Equinix LS1 or Interxion)
- **Type**: Colocation (full rack)
- **Components**: Gateway, Captor, Safe (hot storage)

**Disaster Recovery Site**:
- **Location**: Porto or secondary Lisbon facility
- **Type**: Colocation (half rack) or dedicated servers
- **Components**: Standby Gateway, Captor, Safe replica

**Why This Configuration**:
- ✅ Meets SRIJ requirement (both in Portugal)
- ✅ Premium network connectivity (Lisbon)
- ✅ Geographic redundancy (Lisbon/Porto)
- ✅ Meets <4 hour downtime requirement (automated DR)
- ✅ ISO 27001 certified facilities
- ✅ 24/7 support
- ✅ Scalable for growth

---

### 12.2 Decision Flowchart

```
Start: Do you need SRIJ-compliant hosting?
│
├─ Yes → Must be in Portugal (mandatory)
│   │
│   ├─ Large operator (>50,000 players)?
│   │   │
│   │   ├─ Yes → Equinix or Interxion Lisbon (Tier III)
│   │   │         + DR site in Porto
│   │   │
│   │   └─ No → Continue to next question
│   │
│   ├─ Need managed services?
│   │   │
│   │   ├─ Yes → Claranet Portugal or PTIN (managed hosting)
│   │   │
│   │   └─ No → Continue to next question
│   │
│   ├─ Budget-conscious?
│   │   │
│   │   ├─ Yes → Local Portuguese provider (Lusavouga)
│   │   │         OR dedicated servers from PTIN/Vodafone
│   │   │
│   │   └─ No → Equinix/Interxion (best compliance and network)
│   │
│   └─ Final Step: Contact SRIJ to confirm provider acceptability
│
└─ No → (This guide does not apply)
```

---

## 13. Hosting Checklist

Use this checklist when evaluating and selecting your Portuguese data center:

### Pre-Selection
- [ ] Confirmed SRIJ requirement for Portugal hosting
- [ ] Contacted SRIJ for approved/recommended providers
- [ ] Obtained SRIJ IP addresses for latency testing
- [ ] Defined budget for hosting
- [ ] Determined primary and DR site needs

### Provider Evaluation
- [ ] Requested quotes from minimum 3 providers
- [ ] Verified physical location is in Portugal
- [ ] Confirmed ISO 27001 certification
- [ ] Verified Tier II or higher classification
- [ ] Tested network latency to SRIJ infrastructure
- [ ] Reviewed SLA terms (uptime, response time)
- [ ] Confirmed 24/7 Portuguese-speaking support
- [ ] Verified ability to provision SRIJ connection
- [ ] Checked references from other gambling/regulated clients

### Site Visit (Recommended for Primary Site)
- [ ] Scheduled visit to top 2-3 finalists
- [ ] Inspected physical security (guards, biometrics, cameras)
- [ ] Reviewed power and cooling systems
- [ ] Met support team
- [ ] Verified network operations center (NOC)
- [ ] Checked rack space availability and quality
- [ ] Confirmed cross-connect and meet-me room access

### Contract Negotiation
- [ ] Reviewed contract terms and minimum commitment
- [ ] Negotiated pricing and included services
- [ ] Clarified SLA penalties and credits
- [ ] Confirmed SRIJ facility access policy (for inspections)
- [ ] Included disaster recovery provisions
- [ ] Verified contract flexibility for scaling

### Post-Selection
- [ ] Signed contract
- [ ] Ordered rack space and connectivity
- [ ] Coordinated hardware delivery (if colocation)
- [ ] Requested SRIJ connection provisioning
- [ ] Set up remote access (VPN, jump host)
- [ ] Configured monitoring and alerting
- [ ] Documented all provider contacts and procedures

---

## 14. Summary and Action Items

### Key Takeaways

1. ✅ **Gateway and Safe MUST be in Portugal** - This is a legal requirement
2. ✅ **Lisbon is recommended** - Best infrastructure and SRIJ proximity
3. ✅ **Use Tier III providers** - Equinix or Interxion for maximum compliance
4. ✅ **Plan for DR** - Porto or secondary Lisbon site for redundancy
5. ✅ **Contact SRIJ first** - Confirm provider acceptability before signing

### Immediate Next Steps

1. **Contact SRIJ**:
   - Request list of approved data center providers
   - Obtain SRIJ infrastructure IP addresses
   - Clarify any specific hosting requirements

2. **Get Quotes**:
   - Equinix Lisbon
   - Interxion Lisbon
   - Claranet Portugal
   - PTIN or Vodafone Portugal

3. **Evaluate Options**:
   - Compare pricing, SLAs, and services
   - Test network latency to SRIJ
   - Check compliance certifications

4. **Site Visits**:
   - Visit top 2 finalists in Lisbon
   - Inspect facilities and meet teams

5. **Make Decision**:
   - Select primary provider
   - Select DR provider (if different)
   - Negotiate and sign contracts

6. **Provision Infrastructure**:
   - Order racks and connectivity
   - Set up SRIJ connection
   - Deploy servers and begin implementation

---

## Document Information

**Version**: 1.0
**Last Updated**: 2025-11-10
**Purpose**: Guide for selecting compliant hosting in Portugal for SRIJ ERI
**Status**: Comprehensive hosting requirements documentation

---

**DISCLAIMER**: This document provides guidance based on publicly available SRIJ requirements and market research. Always verify hosting requirements directly with SRIJ before making final decisions. Provider information is accurate as of the document date but should be verified with providers directly. Pricing and offerings may change.

**Next Document**: See DEPLOYMENT_CHECKLIST.md for step-by-step deployment procedures.
