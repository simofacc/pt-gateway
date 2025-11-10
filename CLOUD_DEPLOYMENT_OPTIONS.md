# SRIJ ERI Gateway - Cloud Deployment Options in Portugal

## Overview

This document evaluates using cloud infrastructure (AWS, Google Cloud, Azure, or local Portuguese cloud providers) to deploy your SRIJ ERI gateway while meeting the **mandatory Portugal hosting requirement**.

**CRITICAL**: SRIJ requires that Gateway, Captor, and Safe infrastructure be **physically located in Portuguese territory**. This is a legal requirement.

---

## Current Cloud Provider Availability in Portugal (2025)

### Major Global Cloud Providers

#### ❌ **AWS** - No Full Region in Portugal
**Status**: No AWS region in Portugal as of 2025

**What AWS Has in Portugal**:
- ✅ **AWS Direct Connect** location at Equinix LS1 Lisbon (opened 2025)
  - 10 Gbps and 100 Gbps connections
  - MACsec encryption available
  - Connect to Ireland (eu-west-1) or Spain regions
- ✅ **CloudFront Edge** location in Lisbon (CDN/caching only)
- ❌ **No Compute, Storage, or Database services** physically in Portugal

**For SRIJ Compliance**: ❌ **Cannot use** - compute/storage must be in Portugal, not Ireland

---

#### ❌ **Google Cloud** - No Region in Portugal
**Status**: No Google Cloud region in Portugal as of 2025

**What Google Has Planned**:
- 🔄 **Submarine cable** landing near Lisbon (expected 2026)
- 🔄 **Azores datacenter** project (future, not for commercial cloud)
- ❌ **No compute region** in mainland Portugal

**Closest Regions**:
- europe-southwest1 (Madrid, Spain)
- europe-west1 (Belgium)

**For SRIJ Compliance**: ❌ **Cannot use** - no infrastructure in Portugal

---

#### ❌ **Microsoft Azure** - No Region in Portugal
**Status**: No Azure region in Portugal as of 2025

**Closest Azure Regions**:
- West Europe (Netherlands)
- France Central (Paris)

**Azure in Portugal**:
- Portuguese customers can use Azure services via European regions
- No physical datacenter or region in Portugal

**For SRIJ Compliance**: ❌ **Cannot use** - no infrastructure in Portugal

---

### ✅ Portuguese Local Cloud Providers

#### **Portuguese IaaS/Cloud Options That Meet SRIJ Requirements**

Since major global cloud providers don't have regions in Portugal, you have these options:

---

## Option 1: Portuguese Cloud Providers (✅ SRIJ Compliant)

### 1. **Out.Cloud** (Recommended Portuguese Cloud)
**Website**: https://out.cloud
**Locations**: Lisbon and Porto
**Type**: DevOps-driven cloud consultancy and managed services

**Services**:
- IaaS (Infrastructure as a Service)
- PaaS (Platform as a Service)
- Managed cloud services
- Kubernetes hosting
- Data residency guaranteed in Portugal

**SRIJ Compliance**: ✅ **YES** - infrastructure physically in Portugal

**Pros**:
- ✅ Portuguese data sovereignty
- ✅ Local support (Portuguese-speaking)
- ✅ GDPR compliant by design
- ✅ Can host VMs for Gateway, Captor, Safe
- ✅ Managed services available

**Cons**:
- Smaller scale than AWS/GCP/Azure
- May have less mature tooling/APIs
- Less documentation than major clouds

**Cost**: Contact for quote (typically competitive with colocation)

---

### 2. **Vawlt**
**Focus**: Data sovereignty and security
**Type**: Portuguese cloud provider

**Services**:
- Secure cloud hosting
- Strong focus on data protection
- Ideal for sensitive/regulated data

**SRIJ Compliance**: ✅ **YES** - Portuguese infrastructure

**Best For**: Organizations prioritizing data sovereignty and security

---

### 3. **Flipkick**
**Locations**: Porto region
**Type**: Cloud and DevOps consultancy for SMEs

**Services**:
- Cloud hosting via Portuguese infrastructure
- DevOps services
- Local data residency

**SRIJ Compliance**: ✅ **YES** - Portuguese infrastructure

**Best For**: Smaller operators and SMEs

---

### 4. **Portuguese VPS Providers**

**Several Portuguese VPS hosting providers** offer virtual servers in Portugal:
- Local datacenters (Lisbon, Porto)
- GDPR compliant
- Data residency in Portugal

**SRIJ Compliance**: ✅ **YES** if verified to be physically in Portugal

---

## Option 2: Colocation + Self-Managed Cloud (✅ SRIJ Compliant, Recommended)

### **DIY Cloud with Colocation**

Instead of using a managed cloud provider, deploy your own "private cloud" infrastructure in a Portuguese datacenter:

**Approach**:
1. **Rent rack space** at Equinix/Interxion Lisbon
2. **Deploy servers** (physical or run your own virtualization)
3. **Install virtualization layer**:
   - **OpenStack** (open source private cloud)
   - **Proxmox VE** (open source virtualization)
   - **VMware vSphere** (commercial)
   - **KVM + libvirt** (lightweight)
4. **Self-manage** your "cloud" infrastructure

**Benefits**:
- ✅ Full control (like cloud)
- ✅ Cost-effective for fixed workloads
- ✅ SRIJ compliant (Portugal datacenter)
- ✅ Can deploy VMs on-demand
- ✅ No vendor lock-in

**Drawbacks**:
- More management overhead
- No pay-as-you-go (fixed costs)
- You manage hardware failures

**Cost**:
- Colocation: €2,500-€6,000/month (full rack)
- Servers: €70,000 one-time
- Virtualization software: €0-€10,000 (depends on choice)

---

## Option 3: Hybrid - AWS/GCP/Azure + Portugal Extension (⚠️ Complex)

### **Use Global Cloud with Portugal Data Residency**

**Approach**:
Run most of your infrastructure on AWS/GCP/Azure in nearby regions (Ireland, Spain), but deploy the **SRIJ-required components** (Gateway, Captor, Safe) in Portugal.

**Architecture**:
```
┌─────────────────────────────────────────────────┐
│  AWS eu-west-1 (Ireland) or GCP Madrid         │
│                                                 │
│  - Main gaming platform                        │
│  - Player database (non-SRIJ data)            │
│  - Payment processing                          │
│  - CRM and marketing                           │
│                                                 │
└────────────────┬────────────────────────────────┘
                 │
                 │ AWS Direct Connect /
                 │ VPN Tunnel
                 │
                 ▼
┌─────────────────────────────────────────────────┐
│  Portugal (Equinix/Interxion Lisbon)           │
│  Physical servers or Portuguese cloud          │
│                                                 │
│  - Gateway (SRIJ requirement)                  │
│  - Captor (SRIJ requirement)                   │
│  - Safe (SRIJ requirement)                     │
│  - pfSense VPN gateway                         │
│                                                 │
└─────────────────┬───────────────────────────────┘
                  │
                  │ IPsec VPN
                  ▼
           ┌──────────────┐
           │    SRIJ      │
           └──────────────┘
```

**Benefits**:
- ✅ Use AWS/GCP/Azure for most services
- ✅ SRIJ-compliant (Portugal components)
- ✅ Leverage cloud scalability for gaming platform

**Drawbacks**:
- Complex architecture
- Latency between Ireland/Spain and Portugal
- Still need Portugal infrastructure (no cost savings on ERI)
- AWS Direct Connect fees

**Cost**:
- AWS/GCP/Azure: Variable (pay-as-you-go)
- Portugal infrastructure: Same as colocation (~€50k+/year)
- AWS Direct Connect: €0.30/GB transfer + €2,200/month port fee

---

## Detailed Analysis: Can You Use Cloud for SRIJ?

### What SRIJ Requires in Portugal

| Component | Must Be in Portugal? | Can Use Cloud? |
|-----------|---------------------|----------------|
| **Gateway** | ✅ YES (legal requirement) | ✅ YES (Portuguese cloud) |
| **Captor** | ✅ YES (legal requirement) | ✅ YES (Portuguese cloud) |
| **Safe** | ✅ YES (legal requirement) | ⚠️ MAYBE (see below) |
| **pfSense VPN** | ✅ YES (connects to SRIJ) | ⚠️ MAYBE (see below) |
| Gaming Platform | ❌ NO (can be anywhere) | ✅ YES (any cloud) |
| Player Database | ❌ NO (can be anywhere) | ✅ YES (any cloud) |

---

### SRIJ-Specific Considerations for Cloud

#### 1. **Safe Storage Requirements**
**Challenge**: Safe must store 10 years of data with specific access patterns

**Cloud Considerations**:
- ✅ **Hot storage** (24 months): Can use cloud block storage (SSD)
  - Portuguese cloud provider: Block storage volumes
  - Cost: ~€0.10-€0.20/GB/month
  - 1TB = €100-€200/month

- ⚠️ **Cold storage** (96 months): Cloud object storage or archival
  - Portuguese cloud: Object storage (S3-compatible)
  - Cost: ~€0.01-€0.05/GB/month
  - 10TB = €100-€500/month

- ❌ **FTPS access for SRIJ**: Cloud providers typically don't offer direct FTPS
  - **Workaround**: Run FTPS server on cloud VM with attached storage

**Recommendation**:
- Use cloud VMs with attached block storage (hot)
- Use cloud object storage or separate archive (cold)
- Run vsftpd/ProFTPD on VM for SRIJ FTPS access

---

#### 2. **VPN Tunnel to SRIJ**
**Challenge**: SRIJ requires IPsec site-to-site VPN tunnel

**Cloud Considerations**:
- ❌ **Managed VPN services** (AWS VPN, GCP Cloud VPN, Azure VPN): Not in Portugal
- ✅ **VM-based VPN**: Run pfSense VM in Portuguese cloud
  - Deploy pfSense or strongSwan on cloud VM
  - Terminate SRIJ IPsec tunnel on this VM
  - Works well with Portuguese cloud providers

**Recommendation**:
- Deploy pfSense VM in Portuguese cloud
- Configure IPsec tunnel as documented in VPN_TUNNEL_ARCHITECTURE.md
- Treat cloud VM like physical appliance

---

#### 3. **NTP Synchronization**
**Challenge**: Must sync with Lisbon Astronomical Observatory

**Cloud Considerations**:
- ✅ Easy to configure on cloud VMs
- No different from physical servers
- Configure NTP client to point to `ntp.oal.ul.pt`

---

#### 4. **SRIJ Access and Compliance**
**Question**: Will SRIJ accept cloud-hosted infrastructure?

**Considerations**:
- SRIJ requires **physical presence in Portugal** ✅
- SRIJ requires **permanent FTPS access** ✅ (via VM)
- SRIJ may conduct **physical inspections** ⚠️
  - Cloud providers can provide datacenter access
  - But you don't control physical hardware

**IMPORTANT**: ⚠️ **Verify with SRIJ** if they accept cloud-hosted infrastructure (VMs) vs. dedicated physical servers

**Likely SRIJ response**:
- ✅ Acceptable if infrastructure is physically in Portugal
- ✅ Acceptable if SRIJ has access (FTPS works)
- ⚠️ May require proof of datacenter location
- ⚠️ May require audit rights to cloud provider

---

## Recommended Cloud Architectures

### Architecture 1: Pure Portuguese Cloud (Simplest)

```
Portuguese Cloud Provider (Out.Cloud / Vawlt)
├── VPC/Virtual Network (10.0.0.0/16)
│
├── Public Subnet (10.0.1.0/24)
│   ├── pfSense VM (VPN gateway)
│   │   └── Public IP → SRIJ IPsec tunnel
│   └── Gateway VMs (×2)
│       └── Load balancer
│
├── Private Subnet - App (10.0.2.0/24)
│   └── Captor VMs (×2)
│
├── Private Subnet - Storage (10.0.3.0/24)
│   ├── Safe VM (FTPS server)
│   └── Block Storage (24-month hot data)
│
└── Archive Storage
    └── Object Storage (96-month cold data)
```

**Management**:
- Cloud provider's console/API
- SSH access to VMs
- Similar to AWS/GCP/Azure experience

**Cost** (estimated for 10,000 active players):
- VMs: €500-€1,500/month (depends on sizing)
- Storage: €200-€400/month (hot + cold)
- Bandwidth: €100-€300/month
- **Total**: €800-€2,200/month (~€10k-€26k/year)

---

### Architecture 2: Hybrid AWS + Portuguese Infrastructure

```
┌───────────────────────────────────────────┐
│  AWS eu-west-1 (Ireland)                  │
│  ┌─────────────────────────────────────┐  │
│  │  Gaming Platform                    │  │
│  │  - EC2 for game servers             │  │
│  │  - RDS for player database          │  │
│  │  - ElastiCache for sessions         │  │
│  │  - S3 for assets                    │  │
│  └─────────────────────────────────────┘  │
└───────────────┬───────────────────────────┘
                │
                │ AWS Direct Connect
                │ (via Equinix LS1)
                │
┌───────────────▼───────────────────────────┐
│  Equinix LS1 Lisbon (Colocation)          │
│  ┌─────────────────────────────────────┐  │
│  │  SRIJ ERI Infrastructure            │  │
│  │  - pfSense (VPN + firewall)         │  │
│  │  - Gateway servers (physical)       │  │
│  │  - Captor servers                   │  │
│  │  - Safe servers + storage           │  │
│  └─────────────────────────────────────┘  │
└───────────────┬───────────────────────────┘
                │
                │ IPsec VPN
                ▼
         ┌──────────────┐
         │    SRIJ      │
         └──────────────┘
```

**Benefits**:
- Use AWS for gaming (scalability, managed services)
- SRIJ compliance with Portugal physical infrastructure
- AWS Direct Connect for low-latency connection

**Cost**:
- AWS: Variable (€2k-€10k+/month depending on scale)
- Portugal colocation: €4k-€8k/month
- Direct Connect: €2.2k/month + €0.30/GB
- **Total**: €8k-€20k+/month

---

### Architecture 3: Portuguese Cloud + External Gaming Platform

```
┌────────────────────────────────────────┐
│  External Gaming Platform              │
│  (AWS, GCP, Azure, or your own DC)    │
│  - Game servers                        │
│  - Player database                     │
└────────────────┬───────────────────────┘
                 │
                 │ HTTPS/VPN
                 │
┌────────────────▼────────────────────────┐
│  Portuguese Cloud (Out.Cloud)          │
│  ┌──────────────────────────────────┐  │
│  │  SRIJ ERI Infrastructure         │  │
│  │  - pfSense VM                    │  │
│  │  - Gateway VMs                   │  │
│  │  - Captor VMs                    │  │
│  │  - Safe VM + storage             │  │
│  └──────────────────────────────────┘  │
└────────────────┬────────────────────────┘
                 │
                 │ IPsec VPN
                 ▼
          ┌──────────────┐
          │    SRIJ      │
          └──────────────┘
```

**Benefits**:
- Keep gaming platform where it is
- Add SRIJ-compliant Portuguese infrastructure
- Simpler than hybrid AWS

**Cost**:
- Portuguese cloud: €800-€2.2k/month
- Gaming platform: Your existing costs
- **Total**: Existing + €10k-€26k/year for ERI

---

## Cost Comparison: Cloud vs. Colocation

| Option | Year 1 | Recurring/Year | Control | Complexity |
|--------|--------|----------------|---------|------------|
| **Pure Colocation** (physical servers) | €130k | €57k | 100% | Medium |
| **Portuguese Cloud** (Out.Cloud/Vawlt) | €12k-€30k | €12k-€30k | 80% | Low |
| **DIY Cloud** (Proxmox on colocation) | €135k | €60k | 100% | High |
| **Hybrid AWS + Portugal** | €100k+ | €100k+ | 70% | High |
| **Global Cloud (AWS Ireland)** | N/A | N/A | N/A | ❌ Not compliant |

**Recommendation**:
- **For simplicity**: Portuguese cloud provider (Out.Cloud)
- **For control**: Pure colocation with physical servers
- **For hybrid needs**: AWS gaming + Portuguese colocation ERI

---

## Decision Matrix

### Use Portuguese Cloud Provider IF:
✅ You want cloud-like experience (VMs on-demand, APIs)
✅ You want lower upfront costs
✅ You want managed services (backups, monitoring)
✅ You're okay with 80% control
✅ Your scale is small to medium (< 50,000 players)
✅ SRIJ accepts cloud-hosted VMs (verify!)

### Use Colocation IF:
✅ You want 100% control
✅ You want dedicated hardware (no noisy neighbors)
✅ Your scale is large (> 50,000 players)
✅ You need guaranteed performance
✅ You prefer capex over opex
✅ You want to deploy pfSense appliance (recommended in VPN_TUNNEL_ARCHITECTURE.md)

### Use Hybrid (AWS/GCP + Portugal) IF:
✅ Your gaming platform is already on AWS/GCP
✅ You want to leverage managed cloud services
✅ You can afford higher costs
✅ You're okay with complex architecture

---

## CRITICAL: Verify with SRIJ

Before committing to any cloud solution, you **MUST** verify with SRIJ:

**Questions to Ask SRIJ**:

1. ✅ **Physical Location**:
   - "Do you accept virtual machines hosted in Portuguese datacenters?"
   - "Or must it be dedicated physical servers?"

2. ✅ **Cloud Providers**:
   - "Do you have a list of approved cloud providers?"
   - "Are Portuguese cloud providers like Out.Cloud acceptable?"

3. ✅ **Audit Rights**:
   - "How do physical inspections work with cloud-hosted infrastructure?"
   - "Do you need to audit the cloud provider's datacenter?"

4. ✅ **FTPS Access**:
   - "Can FTPS be provided via a VM, or must it be physical server?"

5. ✅ **Precedent**:
   - "Have any operators successfully used cloud infrastructure for ERI?"
   - "Can you provide references?"

**SRIJ Contact**: https://www.srij.turismodeportugal.pt

---

## Recommended Approach: Portuguese Cloud with Colocation Option

### Phase 1: Start with Portuguese Cloud (Year 1)

**Why**:
- ✅ Lower initial investment
- ✅ Faster deployment
- ✅ Prove concept and compliance
- ✅ Get SRIJ approval

**Provider**: Out.Cloud or Vawlt

**Architecture**:
- pfSense VM for SRIJ VPN
- Gateway VMs
- Captor VMs
- Safe VM with cloud storage

**Cost**: €12k-€30k/year

---

### Phase 2: Scale to Colocation (Year 2+)

Once you've proven the market and grown:

**Why**:
- Better performance
- Lower cost at scale
- 100% control
- Deploy physical pfSense appliance

**Transition**:
- Rent rack space at Equinix/Interxion
- Deploy physical servers
- Migrate from cloud VMs
- Keep cloud as DR site (optional)

**Cost**: €57k/year + €70k one-time hardware

---

## Implementation Guide: Portuguese Cloud Deployment

### Step 1: Select Provider

**Contact**:
- Out.Cloud: https://out.cloud
- Vawlt: [search for contact]
- Flipkick: [search for contact]

**RFP Questions**:
- Where are your datacenters physically located? (Must be Portugal)
- Can you provide proof of location?
- Do you support custom VMs (pfSense)?
- What virtualization do you use? (KVM, VMware, etc.)
- Can I bring my own firewall/VPN appliance?
- What's your SLA for uptime?
- How do you handle SRIJ audits/inspections?
- Pricing for compute, storage, bandwidth?

---

### Step 2: Architecture Design

**Using VPN_TUNNEL_ARCHITECTURE.md**:
- Deploy pfSense VM (2 vCPU, 4GB RAM)
- Configure IPsec tunnel to SRIJ
- Deploy Gateway VMs behind pfSense
- Deploy Captor and Safe VMs

**Networking**:
- Request public IP for pfSense (SRIJ VPN endpoint)
- Configure virtual networks / VLANs
- Set up firewall rules

---

### Step 3: Deploy and Configure

**Follow DEPLOYMENT_CHECKLIST.md** but with cloud modifications:

- Use cloud console instead of racking servers
- Use cloud block storage instead of physical disks
- Use cloud snapshots for backups
- Everything else is the same

---

### Step 4: SRIJ Testing

- Configure SRIJ VPN tunnel
- Test FTPS access from SRIJ
- Verify compliance
- Get SRIJ approval

---

## Comparison Table: All Options

| Solution | Location | SRIJ Compliant? | Year 1 Cost | Control | Recommended? |
|----------|----------|----------------|-------------|---------|--------------|
| **AWS Ireland** | Ireland | ❌ NO | N/A | N/A | ❌ No |
| **Google Cloud Spain** | Spain | ❌ NO | N/A | N/A | ❌ No |
| **Azure Netherlands** | Netherlands | ❌ NO | N/A | N/A | ❌ No |
| **Out.Cloud (Portuguese)** | Portugal | ✅ YES | €12k-€30k | 80% | ✅ **Yes** (simple) |
| **Vawlt (Portuguese)** | Portugal | ✅ YES | Similar | 80% | ✅ Yes |
| **Equinix Colocation** | Portugal | ✅ YES | €130k | 100% | ✅ **Yes** (control) |
| **DIY Cloud (Proxmox)** | Portugal | ✅ YES | €135k | 100% | ⚠️ Maybe (complex) |
| **Hybrid AWS+PT** | Multi | ✅ YES (ERI) | €100k+ | 70% | ⚠️ Maybe (expensive) |

---

## Summary and Recommendations

### ✅ YES - You Can Use Cloud for SRIJ, BUT:

1. **Must be Portuguese cloud provider** (Out.Cloud, Vawlt, etc.)
   - ❌ AWS/GCP/Azure have no regions in Portugal

2. **Must verify with SRIJ** that cloud VMs are acceptable
   - Some regulators require physical servers

3. **Works well for**: Small to medium operators starting out
   - Lower initial cost (€12k-€30k/year vs €130k year 1)
   - Faster deployment
   - Simpler operations

4. **Consider colocation if**: You want maximum control and performance
   - Physical pfSense appliance (as detailed in VPN_TUNNEL_ARCHITECTURE.md)
   - Dedicated hardware
   - Better economics at scale

---

## Action Plan

### Week 1: Research and Verify

- [ ] Contact SRIJ to verify cloud hosting is acceptable
- [ ] Request quotes from Portuguese cloud providers:
  - [ ] Out.Cloud
  - [ ] Vawlt
  - [ ] Flipkick
- [ ] Also get quotes from colocation providers:
  - [ ] Equinix Lisbon
  - [ ] Interxion Lisbon

### Week 2: Decision

Based on SRIJ response and quotes, decide:

**Option A: Portuguese Cloud** (if SRIJ accepts)
- Lower cost, faster start
- Use Out.Cloud or Vawlt
- Deploy pfSense VM

**Option B: Colocation** (recommended in VPN_TUNNEL_ARCHITECTURE.md)
- Higher initial cost, more control
- Use Equinix or Interxion
- Deploy physical pfSense appliance

**Option C: Hybrid**
- AWS/GCP for gaming + Portugal colocation for ERI
- Most expensive, most complex

---

## Conclusion

**Can you use cloud?** ✅ **YES**, but only **Portuguese cloud providers**

**Best approach**:
1. **Start small**: Portuguese cloud (Out.Cloud) for year 1
2. **Verify compliance**: Get SRIJ approval
3. **Scale up**: Move to colocation in year 2+ if needed

**If you want maximum control from day 1**:
- Follow **VPN_TUNNEL_ARCHITECTURE.md** with physical pfSense at Equinix/Interxion

**If you want simplicity and lower cost**:
- Use **Portuguese cloud** with pfSense VM

Both are SRIJ-compliant if infrastructure is physically in Portugal! ✅

---

**Document Version**: 1.0
**Last Updated**: 2025-11-10

**Related Documents**:
- VPN_TUNNEL_ARCHITECTURE.md (VPN configuration)
- HOSTING_REQUIREMENTS.md (colocation details)
- TECHNICAL_ARCHITECTURE.md (overall architecture)
