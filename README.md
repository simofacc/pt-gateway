# SRIJ ERI Gateway - Complete Implementation Guide

## 🎯 Project Overview

This repository contains comprehensive documentation for building and deploying a fully compliant **Entry and Registry Infrastructure (ERI) Gateway** for online gambling operations in Portugal, as mandated by **SRIJ** (Serviço de Regulação e Inspeção de Jogos).

**Regulatory Authority**: SRIJ - Turismo de Portugal
**Legal Framework**: Regulamento 903-B/2015, Decreto-Lei 66/2015 (RJO)
**Hosting Requirement**: Must be physically located in Portuguese territory
**Timeline**: Approximately 11-12 months from start to production

---

## 📚 Documentation Structure

This repository contains comprehensive documents that cover all aspects of SRIJ ERI gateway implementation:

### Core Documentation

### 1. [SRIJ_ERI_REQUIREMENTS.md](./SRIJ_ERI_REQUIREMENTS.md)
**Complete regulatory and technical requirements**

- Legal hosting requirements (Portugal mandate)
- Gateway component specifications
- Safe (Cofre) infrastructure requirements
- Data retention (10-year minimum)
- Encryption standards (X.509 v3, RFC 5280)
- FTPS connectivity to SRIJ
- NTP time synchronization requirements
- Availability requirements (<4 hours downtime/month)
- Certification requirements
- Financial guarantees (€500k + €100k)

**Read this first** to understand all SRIJ compliance requirements.

---

### 2. [TECHNICAL_ARCHITECTURE.md](./TECHNICAL_ARCHITECTURE.md)
**Detailed technical architecture and implementation guide**

- High-level architecture diagram
- Gateway component design
- Captor (event capture) implementation
- Safe (data storage) architecture
- Network architecture and firewall rules
- Security architecture (defense in depth)
- Monitoring and alerting setup
- Disaster recovery planning
- Scalability and performance optimization
- Technology stack recommendations
- Code examples and configurations

**Use this** to design and build your technical infrastructure.

---

### 3. [HOSTING_REQUIREMENTS.md](./HOSTING_REQUIREMENTS.md)
**Where and how to host your infrastructure in Portugal**

- Mandatory Portugal hosting requirements
- Data center selection criteria
- Portuguese data center providers (evaluated)
- SRIJ connectivity requirements
- Colocation vs. dedicated vs. cloud
- Cost estimates and budgeting
- RFP template for data center selection
- Dual data center strategy (primary + DR)
- Network design for Portuguese hosting

**Use this** to select and contract with a Portuguese data center.

---

### 4. [DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md)
**Step-by-step deployment checklist with timeline**

- Phase 1: Pre-Planning and Regulatory (Weeks 1-4)
- Phase 2: Infrastructure Planning (Weeks 5-8)
- Phase 3: Infrastructure Deployment (Weeks 9-14)
- Phase 4: Application Development (Weeks 5-16)
- Phase 5: Monitoring and Logging (Weeks 13-16)
- Phase 6: Testing (Weeks 17-24)
- Phase 7: Independent Certification (Weeks 25-32)
- Phase 8: SRIJ Homologation (Weeks 33-40)
- Phase 9: Pre-Production (Weeks 41-44)
- Phase 10: Pilot Launch (Weeks 45-48)
- Phase 11: Full Production Launch (Week 49+)
- Phase 12: Ongoing Operations (Continuous)

**Use this** as your project management guide and progress tracker.

---

### VPN & Network Control Documentation

### 5. [VPN_TUNNEL_ARCHITECTURE.md](./VPN_TUNNEL_ARCHITECTURE.md) ⭐ NEW
**Simplified architecture with full VPN control (no hosting provider dependency)**

- **Why this matters**: SRIJ connects via site-to-site IPsec VPN tunnel with Phase 1/Phase 2 parameters
- Deploy your own VPN/firewall appliance (pfSense recommended)
- Complete control over VPN configuration without involving hosting provider
- Detailed IPsec configuration guide
- Hardware recommendations (Netgate pfSense 6100 ~€2,500)
- Network architecture with VPN gateway
- Firewall rules for SRIJ access
- Remote management setup (OpenVPN for admin access)
- Monitoring and troubleshooting
- Cost: €2,750 one-time, minimal recurring

**Use this** if you want full control over SRIJ VPN connectivity without constant hosting provider involvement.

---

### 6. [PFSENSE_QUICK_REFERENCE.md](./PFSENSE_QUICK_REFERENCE.md) ⭐ NEW
**Quick reference guide for pfSense configuration**

- Initial setup steps
- SRIJ IPsec VPN configuration (copy-paste ready)
- Firewall rules for SRIJ traffic
- Verification and testing procedures
- Troubleshooting common issues
- Useful commands and shell access
- Configuration backup procedures

**Use this** as a quick reference when configuring your pfSense appliance for SRIJ.

---

### 7. [CLOUD_DEPLOYMENT_OPTIONS.md](./CLOUD_DEPLOYMENT_OPTIONS.md) ⭐ NEW
**Can you use AWS, Google Cloud, or Azure? What about Portuguese cloud providers?**

- **Short answer**: ❌ AWS/Google/Azure have no regions in Portugal ✅ Portuguese cloud providers work!
- Evaluation of major cloud providers (AWS, Google Cloud, Azure availability in Portugal)
- Portuguese local cloud providers (Out.Cloud, Vawlt, Flipkick)
- Cloud vs. colocation cost comparison
- Hybrid architectures (AWS gaming platform + Portugal ERI)
- SRIJ compliance for cloud-hosted VMs
- Implementation guide for Portuguese cloud deployment
- Decision matrix: when to use cloud vs. colocation

**Key findings**:
- AWS/Google Cloud/Azure have NO compute regions in Portugal as of 2025
- Portuguese cloud providers like **Out.Cloud** and **Vawlt** offer SRIJ-compliant hosting
- Cloud option: €12k-€30k/year (lower initial cost, 80% control)
- Colocation option: €130k year 1, €57k/year recurring (100% control)
- **Must verify with SRIJ** if they accept cloud VMs vs. physical servers

**Use this** to decide between cloud (simple, lower cost) vs. colocation (control, recommended).

---

## 🚀 Quick Start Guide

### For First-Time Readers

**Follow these steps in order:**

1. **Read SRIJ_ERI_REQUIREMENTS.md** (30-45 minutes)
   - Understand all legal and technical requirements
   - Note critical requirements (Portugal hosting, .pt domain, 10-year retention)
   - Identify areas where you need more information

2. **Contact SRIJ** (Week 1)
   - Website: https://www.srij.turismodeportugal.pt
   - Request technical documentation, Data Model, Homologation Manual
   - Ask for list of approved data centers and testing laboratories
   - Obtain Multicert Public Key for encryption

3. **Review HOSTING_REQUIREMENTS.md** (20-30 minutes)
   - Understand where you must host (Portugal only)
   - Review recommended data center providers
   - Prepare RFP for data center selection

4. **Study TECHNICAL_ARCHITECTURE.md** (1-2 hours)
   - Understand the complete technical architecture
   - Review technology stack options
   - Customize architecture for your player volume

5. **Use DEPLOYMENT_CHECKLIST.md** (Ongoing)
   - Print or create project management tracker
   - Work through each phase systematically
   - Track milestones and completion dates

---

## ✅ Key Requirements Summary

### MUST Be in Portugal 🇵🇹
- ✅ Gateway infrastructure
- ✅ Safe (data storage) infrastructure
- ✅ Captor (event capture) infrastructure
- ✅ Physical data center location

### MUST Have .pt Domain
- ✅ All Portuguese player access via .pt top-level domain
- ✅ Register domain with DNS.pt

### MUST Provide SRIJ Access
- ✅ Permanent FTPS access to Safe for SRIJ
- ✅ Minimum 20 Mbps dedicated connection to SRIJ

### MUST Retain Data
- ✅ 10 years total (120 months)
- ✅ 24 months immediately accessible (hot storage)
- ✅ 96 months in archive (cold storage)

### MUST Be Certified
- ✅ Independent laboratory certification (SRIJ-recognized)
- ✅ SRIJ homologation testing
- ✅ RNG certification

### MUST Synchronize Time
- ✅ NTP with Lisbon Astronomical Observatory

### Maximum Downtime
- ✅ 4 hours per month combined (Captor + Safe)

### Data Security
- ✅ Sign, compress, encrypt all data
- ✅ X.509 v3 / RFC 5280 encryption standards
- ✅ SRIJ Multicert Public Key

---

## 💰 Budget Expectations

### One-Time Costs
- Hardware: €70,000 - €150,000
- Independent testing lab: €50,000
- Legal and consulting: €20,000 - €50,000
- Data center setup: €5,000 - €20,000
- **Total One-Time**: €145,000 - €270,000

### Held Funds (Guarantees)
- Player liability guarantee: €500,000
- Tax payment guarantee: €100,000
- **Total Held**: €600,000

### Annual Recurring Costs
- Data center hosting: €30,000 - €60,000
- Network bandwidth: €6,000 - €12,000
- Software licenses: €10,000 - €15,000
- Personnel (DevOps, security, admin): €150,000 - €240,000
- Ongoing audits and compliance: €10,000 - €20,000
- **Total Annual**: €206,000 - €347,000

**First Year Total (including one-time)**: €951,000 - €1,217,000
**Subsequent Years**: €206,000 - €347,000 annually

*Note: Costs vary significantly based on player volume and vendor selection.*

---

## ⏱️ Timeline

### Critical Path Timeline

```
Month 1:   SRIJ contact, team assembly, planning
Month 2:   Data center selection, hardware procurement
Month 3-4: Infrastructure deployment, OS installation
Month 2-4: Application development (parallel)
Month 5-6: Integration, testing begins
Month 7-8: Independent lab certification
Month 9-10: SRIJ homologation and approval
Month 11:  Pre-production and pilot launch
Month 12:  Full production launch

Total: 11-12 months
```

### Key Milestones

| Milestone | Target | Critical? |
|-----------|--------|-----------|
| SRIJ initial engagement | Week 1 | ✅ Yes |
| Data center contract signed | Week 8 | ✅ Yes |
| Infrastructure deployed | Week 14 | ✅ Yes |
| Application development complete | Week 16 | Yes |
| Independent lab certification | Week 32 | ✅ Yes |
| SRIJ license approved | Week 40 | ✅ Yes |
| Production launch | Week 49 | ✅ Yes |

---

## 🏗️ High-Level Architecture

```
┌─────────────────────────────────────────┐
│     Portuguese Players (.pt domain)    │
└──────────────┬──────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────────┐
│  GATEWAY (Portugal)                              │
│  - IP geolocation                                │
│  - Traffic routing                               │
│  - Event logging                                 │
└──────────────┬───────────────────────────────────┘
               │
               ├──────────────────┐
               │                  │
               ▼                  ▼
┌──────────────────────┐  ┌──────────────────────┐
│  CAPTOR (Portugal)   │  │  Gaming Platform     │
│  - Event capture     │  │  (Can be external)   │
│  - Data categorize   │  └──────────────────────┘
└──────────┬───────────┘
           │
           ▼
┌────────────────────────────────────────┐
│  SAFE (Portugal)                       │
│  - Sign → Compress → Encrypt           │
│  - 24-month hot storage (SSD)          │
│  - 96-month cold storage (HDD/tape)    │
│  - FTPS server for SRIJ                │
└────────────┬───────────────────────────┘
             │
             │ FTPS
             ▼
     ┌───────────────┐
     │     SRIJ      │
     │  (Regulator)  │
     └───────────────┘
```

---

## 🛠️ Technology Stack Recommendations

### Gateway
- **Load Balancer**: HAProxy / Nginx / AWS ALB
- **Application**: Node.js / Python / Go / Java
- **Geolocation**: MaxMind GeoIP2
- **OS**: Ubuntu 22.04 LTS / RHEL 8 / Oracle Linux 8

### Captor
- **Message Queue**: Apache Kafka / RabbitMQ / Redis
- **Processing**: Custom application (Java/Python/Go)
- **Database**: PostgreSQL (for buffering)

### Safe
- **OS**: Oracle Linux 8+ / RHEL 8+ (SRIJ tested)
- **FTPS Server**: vsftpd / ProFTPD with TLS
- **Encryption**: OpenSSL (X.509 v3 certificates)
- **Storage**: SSD (hot) + HDD/Tape (cold)

### Monitoring
- **Metrics**: Prometheus + Grafana
- **Logging**: ELK Stack (Elasticsearch, Logstash, Kibana)
- **Security**: SIEM (Wazah / Splunk / ELK)

### Security
- **IDS/IPS**: Snort / Suricata
- **Firewall**: iptables / firewalld / pfSense
- **DDoS**: Cloudflare / Akamai / data center DDoS protection

---

## 📞 Important Contacts

### SRIJ (Regulatory Authority)
- **Website**: https://www.srij.turismodeportugal.pt
- **Parent Organization**: Turismo de Portugal
- **Address**: Rua Ivone Silva, Lote 6, 1050-124 Lisboa, Portugal
- **Check website for**: Current contact email, phone, and technical documentation

### Portuguese Domain Registry
- **Website**: https://www.dns.pt
- **Purpose**: Register .pt domain

### Time Synchronization
- **NTP Source**: Lisbon Astronomical Observatory
- **Server**: ntp.oal.ul.pt (verify with SRIJ)

---

## ⚠️ Critical Success Factors

### Do These Things Right

1. ✅ **Engage SRIJ Early** - Contact them in Week 1, maintain continuous communication
2. ✅ **Choose the Right Data Center** - Tier III in Lisbon (Equinix or Interxion recommended)
3. ✅ **Hire Experienced Team** - DevOps and security specialists with gambling/compliance experience
4. ✅ **Test Thoroughly** - Don't skip testing phases; SRIJ homologation is rigorous
5. ✅ **Document Everything** - SRIJ requires extensive technical documentation
6. ✅ **Plan for DR** - Disaster recovery is critical for <4 hour downtime requirement
7. ✅ **Budget Realistically** - Add 20% contingency; costs can exceed estimates
8. ✅ **Allow Sufficient Time** - 11-12 months is realistic; rushing increases failure risk

### Common Pitfalls to Avoid

1. ❌ **Don't skip SRIJ engagement** - Assuming requirements without confirmation
2. ❌ **Don't choose non-Portugal hosting** - This is a legal requirement, no exceptions
3. ❌ **Don't underestimate timeline** - 6 months is not realistic for full compliance
4. ❌ **Don't skip independent testing** - SRIJ requires certified lab approval
5. ❌ **Don't forget .pt domain** - Must be registered and operational before launch
6. ❌ **Don't neglect DR planning** - Required for 4-hour downtime limit
7. ❌ **Don't skimp on encryption** - SRIJ verifies X.509 v3 compliance
8. ❌ **Don't ignore data model** - Must match SRIJ specifications exactly

---

## 🔐 Security Highlights

### Multi-Layer Security

1. **Network Perimeter**: DDoS protection, WAF, geographic filtering
2. **Network Security**: Firewall, IDS/IPS, network segmentation
3. **Host Security**: OS hardening, host firewall, antivirus, file integrity
4. **Application Security**: Input validation, authentication, authorization
5. **Data Security**: Encryption at rest and in transit, digital signatures

### Compliance Standards

- ISO 27001 (Information Security Management)
- GDPR (Data Protection)
- X.509 v3 and RFC 5280 (Encryption)
- CIS Benchmarks (OS Hardening)

---

## 📊 Monitoring and Compliance

### Real-Time Monitoring

- System health (CPU, memory, disk, network)
- Application performance (request rate, latency, errors)
- Security events (intrusions, anomalies, alerts)
- SRIJ connectivity and data transfers
- **Downtime tracking** (critical: <4 hours/month)

### Compliance Reporting

- Monthly SRIJ reports (automated generation)
- SRIJ audit logs (all FTPS access)
- Data retention verification (10-year capability)
- Uptime reports (99.5%+ target)

---

## 🆘 Getting Help

### When You Need Assistance

1. **SRIJ Technical Questions**
   - Contact SRIJ directly via their website
   - Request technical specifications and clarifications
   - Ask for list of approved vendors and testing labs

2. **Legal and Regulatory**
   - Hire Portuguese legal counsel specializing in gambling law
   - Consult with gambling compliance experts
   - Engage consultants with SRIJ experience

3. **Technical Implementation**
   - Hire experienced DevOps and security engineers
   - Consider engaging gambling technology consultants
   - Use data center professional services

4. **Testing and Certification**
   - Work with SRIJ-recognized independent testing laboratories
   - Engage security testing firms for penetration testing

---

## 📋 Pre-Flight Checklist

### Before You Begin

Use this checklist to determine if you're ready to start:

- [ ] Committed to 11-12 month timeline
- [ ] Budget of €1M+ available (including guarantees)
- [ ] Understanding of Portuguese gambling regulations
- [ ] Team identified or ready to hire
- [ ] Legal counsel identified (Portuguese gambling law)
- [ ] Willing to host 100% in Portugal (non-negotiable)
- [ ] Willing to obtain independent lab certification
- [ ] Willing to provide SRIJ with permanent infrastructure access
- [ ] Committed to 10-year data retention
- [ ] Understanding that main gaming platform can be outside Portugal
- [ ] Ready to register .pt domain
- [ ] Prepared for rigorous SRIJ homologation process

**If all boxes are checked, you're ready to proceed to Phase 1!**

---

## 🎯 Next Steps

### Your Immediate Action Items

1. **Read All Documentation** (2-3 hours)
   - [ ] SRIJ_ERI_REQUIREMENTS.md
   - [ ] TECHNICAL_ARCHITECTURE.md
   - [ ] HOSTING_REQUIREMENTS.md
   - [ ] DEPLOYMENT_CHECKLIST.md

2. **Contact SRIJ** (Week 1)
   - [ ] Request technical documentation package
   - [ ] Obtain Data Model specifications
   - [ ] Get list of approved testing labs and data centers
   - [ ] Clarify timeline and process

3. **Assemble Your Team** (Weeks 1-2)
   - [ ] Project manager
   - [ ] Technical lead (DevOps)
   - [ ] Security specialist
   - [ ] Legal counsel (Portuguese gambling law)

4. **Begin Planning** (Weeks 2-4)
   - [ ] Detailed project plan based on DEPLOYMENT_CHECKLIST.md
   - [ ] Budget refinement
   - [ ] Risk assessment
   - [ ] Vendor identification (data center, testing lab, etc.)

5. **Start Regulatory Process** (Weeks 1-4)
   - [ ] Financial guarantee arrangements
   - [ ] Corporate documentation preparation
   - [ ] .pt domain registration
   - [ ] License application preparation

---

## 📄 Document Versions

| Document | Version | Last Updated | Status |
|----------|---------|--------------|--------|
| README.md | 1.0 | 2025-11-10 | Complete |
| SRIJ_ERI_REQUIREMENTS.md | 1.0 | 2025-11-10 | Complete |
| TECHNICAL_ARCHITECTURE.md | 1.0 | 2025-11-10 | Complete |
| HOSTING_REQUIREMENTS.md | 1.0 | 2025-11-10 | Complete |
| DEPLOYMENT_CHECKLIST.md | 1.0 | 2025-11-10 | Complete |

---

## ⚖️ Legal Disclaimer

**IMPORTANT**: This documentation is based on publicly available information about SRIJ requirements and general industry best practices as of November 2025.

**You MUST**:
- Verify all requirements directly with SRIJ
- Obtain official technical documentation from SRIJ
- Engage Portuguese legal counsel specializing in gambling law
- Confirm all technical specifications before implementation
- Understand that SRIJ requirements may change

**This documentation**:
- Is provided for informational purposes only
- Does not constitute legal advice
- Does not guarantee SRIJ approval
- Should be used as a planning guide, not as definitive requirements
- Must be validated against current SRIJ regulations

**Liability**: The authors of this documentation assume no liability for any outcomes resulting from the use of this information. Always consult official sources and qualified professionals.

---

## 🌟 Project Goals

By following this documentation, you will:

✅ Understand all SRIJ ERI requirements comprehensively
✅ Have a complete technical architecture for implementation
✅ Know exactly where and how to host in Portugal
✅ Have a step-by-step deployment plan with timeline
✅ Be prepared for SRIJ homologation and certification
✅ Build a compliant, secure, and scalable infrastructure
✅ Launch a fully legal online gambling operation in Portugal

---

## 📞 Support and Contributions

### Questions or Improvements?

This is a living document. If you:
- Find errors or outdated information
- Have improvements or additions
- Want to share your implementation experience
- Need clarification on any section

Please contribute or reach out.

---

## 🏁 Final Words

Building a SRIJ-compliant ERI gateway is a significant undertaking requiring:
- **Time**: 11-12 months minimum
- **Investment**: €1M+ total (including guarantees)
- **Expertise**: Technical, legal, and regulatory
- **Commitment**: To compliance and quality

But with proper planning, the right team, and this comprehensive documentation, you can successfully navigate the process and launch a fully compliant online gambling operation in Portugal.

**Good luck with your SRIJ ERI implementation!**

---

**For detailed implementation, proceed to:**
1. [SRIJ_ERI_REQUIREMENTS.md](./SRIJ_ERI_REQUIREMENTS.md) - Complete requirements
2. [TECHNICAL_ARCHITECTURE.md](./TECHNICAL_ARCHITECTURE.md) - Technical design
3. [HOSTING_REQUIREMENTS.md](./HOSTING_REQUIREMENTS.md) - Where to host in Portugal
4. [DEPLOYMENT_CHECKLIST.md](./DEPLOYMENT_CHECKLIST.md) - Step-by-step deployment

---

**Repository**: pt-gateway
**Purpose**: SRIJ ERI Gateway Implementation Guide
**Regulatory Compliance**: Portugal Online Gambling (SRIJ)
**Version**: 1.0
**Last Updated**: 2025-11-10
