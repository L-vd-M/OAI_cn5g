# OAI CN5G — Repository Reference Document

**Project:** OpenAirInterface 5G Core Network (OAI CN5G)  
**Organisation:** OpenAirInterface Software Alliance / Eurecom  
**License:** OAI Public License V1.1 (unless noted otherwise)  
**Document generated:** 2026-03-23  

---

## Overview

OAI CN5G is a 3GPP-compliant implementation of the **5G Core Network (5GC)**,
developed by the OpenAirInterface community. Each Network Function (NF) lives in
its own Git repository. Three additional shared repositories provide common build
infrastructure, CI tooling, and shared C++ source. A federation repository ties
everything together with Docker-Compose deployment recipes and integration tests.

The base GitLab namespace for all repositories is:  
`https://gitlab.eurecom.fr/oai/cn5g/`

---

## Repository Summary Table

| Repository | Abbreviation | 3GPP Role | Branch | Latest Commit |
|---|---|---|---|---|
| oai-cn5g-amf | AMF | Access & Mobility Management | `master` | `4d9b0d3d` (2025-12-09) |
| oai-cn5g-ausf | AUSF | Authentication Server | `master` | `3ce0281a` (2025-12-10) |
| oai-cn5g-lmf | LMF | Location Management | `master` | `4111b2c4` (2025-12-04) |
| oai-cn5g-nef | NEF | Network Exposure | `master` | `bd145f8c` (2023-05-15) |
| oai-cn5g-nrf | NRF | NF Repository / Discovery | `master` | `7ed0bc3b` (2025-12-14) |
| oai-cn5g-nssf | NSSF | Network Slice Selection | `master` | `5d3d152d` (2025-12-14) |
| oai-cn5g-nwdaf | NWDAF | Network Data Analytics | `master` | `791a37fb` (2023-12-14) |
| oai-cn5g-pcf | PCF | Policy Control | `master` | `89845796` (2025-12-08) |
| oai-cn5g-smf | SMF | Session Management | `master` | `29cfc992` (2025-12-14) |
| oai-cn5g-udm | UDM | Unified Data Management | `master` | `c2a21b7c` (2025-12-14) |
| oai-cn5g-udr | UDR | Unified Data Repository | `master` | `0b6dc8d8` (2025-12-12) |
| oai-cn5g-upf | UPF | User Plane Function | `master` | `0d46d595` (2025-12-14) |
| oai-cn5g-upf-vpp | UPF-VPP | User Plane (VPP-based) | `master` | `7f006598` (2023-05-15) |
| oai-cn5g-upf-sdfabric | UPF-SD | User Plane (SD-Fabric) | `master` | `cbc4f8e1` (2023-04-28) |
| oai-cn5g-common-build | — | Shared build scripts | `develop` | `00dc4fd0` (2026-02-05) |
| oai-cn5g-common-ci | — | Shared CI scripts | `develop` | `3407df4f` (2025-08-18) |
| oai-cn5g-common-src | — | Shared C++ source | `develop` | `2cae1fc1` (2026-03-11) |
| oai-cn5g-fed | — | Federation / Deployment | `develop` | `6b44e2be` (2026-03-12) |

> **Current release:** v2.2.0 (December 2025) across all actively maintained NF repos.

---

## Detailed Repository Descriptions

---

### 1. `oai-cn5g-amf` — Access and Mobility Management Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf.git` |
| **Branch** | `master` |
| **Commit** | `4d9b0d3d1090c59f528d2d652ae7d360ef5a886e` (2025-12-09) |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.501):**  
The AMF is the primary control-plane entry point for UEs. It terminates the **N1**
interface (NAS signalling with the UE) and the **N2** interface (NGAP signalling
with the gNB/RAN). It handles:

- UE registration and deregistration
- Authentication coordination with AUSF
- Mobility management (handover, paging, tracking area updates)
- Connection management (CM-IDLE / CM-CONNECTED state machines)
- Session management request relay to SMF
- Policy enforcement coordination with PCF
- Interaction with NRF for NF discovery

**Source structure:**
```
src/
├── amf-app/       Core AMF application logic
├── common/        AMF-specific common headers
├── common-src/    Symlink to oai-cn5g-common-src submodule
├── contexts/      UE and RAN context management
├── itti/          Inter-task interface (message passing framework)
├── nas/           NAS (Non-Access Stratum) protocol implementation
├── ngap/          NGAP (Next Generation Application Protocol) implementation
├── oai-amf/       Main entry point / CMakeLists
├── sbi/           Service-Based Interface (HTTP/2 REST API)
├── sctp/          SCTP transport layer
├── secu_algorithms/ AKA security algorithms
└── utils/         Utility classes
```

---

### 2. `oai-cn5g-ausf` — Authentication Server Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-ausf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-ausf.git` |
| **Branch** | `master` |
| **Commit** | `3ce0281af3a4e1f2854c1711dec88f97356ce78b` (2025-12-10) |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 33.501 / TS 29.509):**  
The AUSF implements the **5G AKA** and **EAP-AKA'** authentication procedures. It
is invoked by the AMF when a UE needs to be authenticated. Key responsibilities:

- Primary authentication and key agreement (5G-AKA)
- EAP-AKA' authentication method support
- Retrieval of authentication vectors from UDM
- SoR (Steering of Roaming) protection
- UPU (UE Parameter Update) protection

**Source structure:**
```
src/
├── 5gaka/         5G AKA authentication algorithm implementation
├── api_server/    SBI REST API server (auto-generated from 3GPP YAML specs)
├── ausf_app/      Core AUSF application logic
├── common/        AUSF-specific utilities
├── common-src/    Shared source submodule
└── oai_ausf/      Main entry point / CMakeLists
```

---

### 3. `oai-cn5g-lmf` — Location Management Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-lmf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-lmf.git` |
| **Branch** | `master` |
| **Commit** | `4111b2c4577ac7655690e52eee1f9c6bcc886920` (2025-12-04) — Release v2.2.0 |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.273 / TS 29.572):**  
The LMF handles **UE positioning and location services** within the 5GC. It:

- Receives location requests from AMF (NLs interface)
- Coordinates positioning methods (NR positioning, ECID, etc.)
- Communicates with the RAN (via AMF) to obtain measurements
- Returns location estimates to requesting entities
- Supports LCS (Location Services) for emergency and commercial use

---

### 4. `oai-cn5g-nef` — Network Exposure Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nef |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nef.git` |
| **Branch** | `master` |
| **Commit** | `bd145f8c044e85e0a7a46444710dce1ecc094efd` (2023-05-15) |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

> **Note:** This repository has not received major updates since May 2023. It
> implements an early version of the NEF.

**Role (3GPP TS 23.502 / TS 29.522):**  
The NEF exposes 5GC capabilities and events to **external Application Functions
(AF)** and third-party applications via secure northbound APIs. It:

- Acts as a gateway/proxy between the 5GC and external applications
- Exposes network events (UE mobility, session events) to AFs
- Provides policy and QoS parameter provisioning interfaces
- Supports traffic influence requests from AFs
- Translates between external (AF-facing) and internal (SBI) interfaces

---

### 5. `oai-cn5g-nrf` — Network Repository Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nrf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nrf.git` |
| **Branch** | `master` |
| **Commit** | `7ed0bc3bde8c5dc302e9995000da1044b84d15c7` (2025-12-14) — v2.2.0 feature set |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.501 / TS 29.510):**  
The NRF is the **service registry and discovery** hub of the 5GC. Every other NF
registers itself with the NRF and queries it to discover peer NFs. Key functions:

- NF instance registration, update, and deregistration
- NF profile storage (NF type, capacity, supported services, IP addresses)
- NF discovery service (respond to queries for specific NF types)
- NF subscription/notification for status changes
- OAuth2-based NF access token issuance (NRF as authorization server)

**Source structure:**
```
src/
├── api-server/    REST API server (auto-generated from 3GPP YAML specs)
├── common/        NRF-specific utilities
├── common-src/    Shared source submodule
├── nrf_app/       Core NRF application / NF profile database
└── oai_nrf/       Main entry point / CMakeLists
```

---

### 6. `oai-cn5g-nssf` — Network Slice Selection Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nssf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nssf.git` |
| **Branch** | `master` |
| **Commit** | `5d3d152d1702fe98dc3810a385ea310189ca1d07` (2025-12-14) — v2.2.0 feature set |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.501 / TS 29.531):**  
The NSSF selects the appropriate **network slice** for a UE based on its
subscription and requested S-NSSAIs. Key functions:

- Network Slice Selection Service (NSSELECTION)
- Network Slice Status Subscription/Notification Service
- Maps requested S-NSSAIs to allowed S-NSSAIs and AMF sets
- Determines which NRF instance is authoritative for a given slice

**Source structure:**
```
src/
├── api-server/    (api/, impl/, model/) REST API server
├── common/        (msg/, utils/) NSSF-specific utilities
├── nssf_app/      Core NSSF application logic / slice config
└── oai_nssf/      Main entry point / CMakeLists
```

---

### 7. `oai-cn5g-nwdaf` — Network Data Analytics Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nwdaf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nwdaf.git` |
| **Branch** | `master` |
| **Commit** | `791a37fb939f3f61c8d0a42e6199e5c3fd581305` (2023-12-14) |
| **Language** | Python (microservices) |
| **License** | OAI Public License V1.1 |

> **Note:** This repository has not received major updates since December 2023.

**Role (3GPP TS 23.288 / TS 29.520):**  
The NWDAF provides **data analytics and machine learning** capabilities within
the 5GC. Unlike other NFs, it is implemented as a **microservices architecture**:

- `oai-nwdaf-sbi`: SBI interface, handles analytics subscription/notification
- `oai-nwdaf-engine`: Core analytics engine
- `oai-nwdaf-engine-ads`: Anomaly Detection Service analytics
- `oai-nwdaf-nbi-analytics`: Northbound analytics API
- `oai-nwdaf-nbi-events`: Northbound events subscription API
- `oai-nwdaf-nbi-ml`: Northbound machine learning model API

Supports UE mobility analytics, load level analytics, network performance analytics,
and slice SLA monitoring.

**Source structure:**
```
components/
├── oai-nwdaf-engine/
├── oai-nwdaf-engine-ads/
├── oai-nwdaf-nbi-analytics/
├── oai-nwdaf-nbi-events/
├── oai-nwdaf-nbi-ml/
└── oai-nwdaf-sbi/
deployment/
docker-compose/
cli/
```

---

### 8. `oai-cn5g-pcf` — Policy Control Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-pcf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-pcf.git` |
| **Branch** | `master` |
| **Commit** | `8984579a22fb75a0ae7a2795d1a923d2adee3157` (2025-12-08) — Release V2.2.0 |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.503 / TS 29.507, 29.512, 29.523):**  
The PCF is the central **policy engine** of the 5GC. It generates and enforces
policy rules for both access and session management. Key functions:

- AM Policy Control (access and mobility policies for AMF)
- SM Policy Control (session and QoS policies for SMF)
- Background Data Transfer Policy
- UE Policy association and delivery
- Policy subscription to UDR for subscriber-specific policy data
- Interaction with AF for application-triggered policies

**Source structure:**
```
src/
├── api-server/    (api/, impl/, model/) REST API
├── common/        PCF-specific utilities
├── common-src/    Shared source submodule
├── pcf_app/       Core PCF application / policy engine
└── oai_pcf/       Main entry point / CMakeLists
```

---

### 9. `oai-cn5g-smf` — Session Management Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-smf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-smf.git` |
| **Branch** | `master` |
| **Commit** | `29cfc992b7cdcee073ae4e6c534664ed7a6d780f` (2025-12-14) — v2.2.0 |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.502 / TS 29.502):**  
The SMF manages the **PDU session lifecycle** — establishment, modification, and
release. It is the control-plane counterpart of the UPF. Key functions:

- PDU session establishment, modification, and release (N11 with AMF)
- PFCP session management with UPF (N4 interface)
- IP address allocation (IPAM) for UE PDU sessions
- QoS enforcement rules pushed to UPF
- Charging data collection and interaction with CHF
- Interaction with PCF for session-level policy
- Integration with UDM for session subscription data
- DNS configuration for UE sessions
- Support for multiple UPF deployments (UL-CL, branching point)

**Source structure:**
```
src/
├── api-server/    SBI REST API (Nsmf services)
├── common/        SMF-specific headers and utils
├── common-src/    Shared source submodule
├── gtpv1u/        GTPv1-U stack (control path)
├── itti/          Inter-task interface
├── oai_smf/       Main entry point / CMakeLists
├── pfcp/          PFCP (Packet Forwarding Control Protocol) implementation
└── smf_app/       Core SMF application / session state machines
```

---

### 10. `oai-cn5g-udm` — Unified Data Management

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm.git` |
| **Branch** | `master` |
| **Commit** | `c2a21b7cb4d0f1c3ebaff11205b4d4ad8fa7b482` (2025-12-14) — v2.2.0 |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.502 / TS 29.503):**  
The UDM provides a **unified view of subscriber data** and generates authentication
credentials. It acts as the front-end to the UDR. Key functions:

- Authentication credential generation (for AUSF — 5G-AKA / EAP-AKA')
- UE subscription data management (access to session, mobility subscriptions)
- MT-SMS delivery and routing
- SUPI/SUCI de-concealment
- UE context management and registration coordination
- Event exposure and subscription for subscriber status

**Source structure:**
```
src/
├── api-server/    SBI REST API (Nudm services)
├── common/        UDM-specific utilities
├── common-src/    Shared source submodule
├── oai_udm/       Main entry point / CMakeLists
└── udm_app/       Core UDM application / credential generation
```

---

### 11. `oai-cn5g-udr` — Unified Data Repository

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr.git` |
| **Branch** | `master` |
| **Commit** | `0b6dc8d884f86c842400fa03df2479be71e3040a` (2025-12-12) — Release v2.2.0 |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.501 / TS 29.504):**  
The UDR is the **data storage backend** for the 5GC. It stores subscriber
profiles, policy data, and application data and exposes them via a RESTful
Nudr interface. Consumers include UDM, PCF, and NEF. Key stored data:

- Subscriber (UE) data (authentication, mobility, session subscriptions)
- Policy data (AM policies, SM policies, UE policies)
- Application data (influence data, PFDs)
- Exposure data (event subscriptions for NEF)

The UDR connects to a **MySQL database** for persistent storage.

---

### 12. `oai-cn5g-upf` — User Plane Function

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf.git` |
| **Branch** | `master` |
| **Commit** | `0d46d5950b0a069bb9715933fae45a56d00910d6` (2025-12-14) — Release V2.2.0 |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Role (3GPP TS 23.501 / TS 29.244):**  
The UPF is the **data-plane anchor** responsible for forwarding user traffic
between the RAN (gNB) and the Data Network (DN/internet). It is controlled by
the SMF via the **PFCP** protocol (N4 interface). Key functions:

- GTPv1-U tunnel termination towards the RAN (N3 interface)
- PDU session traffic forwarding and routing (N6 interface to DN)
- QoS enforcement (policing, gating, marking per PDR/FAR/QER/URR)
- UL/DL packet detection rules (PDR) and forwarding action rules (FAR)
- Usage reporting rules (URR) for charging
- Simple eBPF-accelerated packet switching option
- Buffering for UEs in CM-IDLE state

**Source structure:**
```
src/
├── common/        Common headers and ITTI messages
├── gtpv1u/        GTPv1-U stack implementation
├── gtpv2c/        GTPv2-C stack implementation
├── itti/          Inter-task interface
├── oai_upf/       Main entry point / CMakeLists
│   └── simpleswitch/  Basic packet switch
├── pfcp/          PFCP protocol stack
└── udp/           UDP server
```

---

### 13. `oai-cn5g-upf-vpp` — User Plane Function (VPP-based)

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf-vpp |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf-vpp.git` |
| **Branch** | `master` |
| **Commit** | `7f0065980493ebc49d5e8ce8ca5a9498878c110a` (2023-05-15) |
| **Language** | C / C++ (VPP plugins + patches) |
| **License** | Apache 2.0 |

> **Note:** Last major update May 2023. This repo wraps upstream VPP projects.

**Role:**  
A high-performance alternative UPF implementation based on **VPP (Vector Packet
Processing)** and **UPG-VPP (User Plane Gateway)**. It provides:

- Line-rate packet processing via DPDK/VPP data plane
- Full PFCP support via UPG-VPP plugin
- GTPv1-U encapsulation/decapsulation at wire speed
- Significantly higher throughput than the software UPF for production use

This repository contains **patches and integration scripts** over two upstream
open-source projects:
- [FD.io VPP](https://github.com/fdio/vpp.git)
- [UPG-VPP by Travelping](https://github.com/travelping/upg-vpp)

**Source structure:**
```
scripts/
├── patches/       Patches applied to upstream VPP/UPG-VPP
└── upf_conf/      UPF configuration templates
src/               Integration source code
docker/            Docker build files
docker-compose/    Deployment recipes
```

---

### 14. `oai-cn5g-upf-sdfabric` — User Plane Function (SD-Fabric)

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf-sdfabric |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf-sdfabric.git` |
| **Branch** | `master` |
| **Commit** | `cbc4f8e15f4c37c874db17bef7fbef4d4926b7bf` (2023-04-28) |
| **Language** | Scripts / YAML configuration |
| **License** | Apache 2.0 |

> **Note:** This repository was last updated April 2023. It contains integration
> scripts only — no original NF source code.

**Role:**  
Provides scripts and configuration to run and test the **SD-Fabric UPF** with
the OAI 5G Core Network. SD-Fabric is an SDN-based network fabric for edge cloud
and 5G, built on:

- **PFCP Agent** ([omec-project/upf](https://github.com/omec-project/upf)) — Apache License
- **UP4 App** ([omec-project/up4](https://github.com/omec-project/up4)) — Apache License  
- **ONOS SDN Controller** ([opennetworkinglab/onos](https://github.com/opennetworkinglab/onos)) — Apache License

This repo adds segment routing application support and provides deployment
recipes for integrating SD-Fabric as the data plane for OAI 5GC.

---

### 15. `oai-cn5g-common-build` — Shared Build Infrastructure

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-build |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-build.git` |
| **Branch** | `develop` |
| **Commit** | `00dc4fd07dfe6f404cbbb369319185925215fed0` (2026-02-05) |
| **Language** | CMake / Shell / Python |
| **License** | OAI Public License V1.1 |

**Purpose:**  
A **shared submodule** included in every NF repository under `build/common-build`.
It provides the installation scripts, build system infrastructure, and Docker
entry point that all 5GC network functions share:

- `cmake_modules/`: Custom CMake find-modules (MySQL, FreeDiameter, libEvent, etc.)
- `installation/`: OS-specific dependency installation scripts (Ubuntu, RHEL9, Rocky9)
- `docker-scripts/entrypoint.py`: Common container entrypoint used by all NF Docker images

This repo is automatically updated across all parent repositories when changed.

---

### 16. `oai-cn5g-common-ci` — Shared CI Scripts

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-ci |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-ci.git` |
| **Branch** | `develop` |
| **Commit** | `3407df4f295246ab12718488745d7923c4023f43` (2025-08-18) |
| **Language** | Shell / Python / HTML |
| **License** | OAI Public License V1.1 |

**Purpose:**  
A **shared submodule** included in every NF repository under `ci-scripts/common`.
It provides the Continuous Integration tooling shared across all 5GC NFs:

- `bash/`: Shell utility scripts for CI pipelines
- `docker/`: Docker helper scripts for CI build environments
- `html-templates/`: HTML report generation templates
- `python/`: Python utilities for CI automation and reporting

Also provides automation for propagating commits from this shared submodule to
all parent NF repositories.

---

### 17. `oai-cn5g-common-src` — Shared C++ Source

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-src |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-src.git` |
| **Branch** | `develop` |
| **Commit** | `2cae1fc1633b83f1c77eba0b4601d785a03ebee3` (2026-03-11) |
| **Language** | C++ |
| **License** | OAI Public License V1.1 |

**Purpose:**  
The most actively developed shared repository. Contains **C++ source code shared
by multiple NFs**, included as a Git submodule (`src/common-src`) in each NF
repo. Provides:

- `common/`: Base utilities (logging, string helpers, network utilities)
- `config/`: Common configuration parsing framework (YAML-based)
- `event_handling/`: Async event loop infrastructure
- `http/`: HTTP/2 client and server base classes (libcurl / pistache)
- `itti/`: Inter-Task Interface — the internal message-passing framework
- `logger/`: Structured logging subsystem (spdlog-based)
- `model/`: 3GPP data model classes shared across NFs
- `nas/`: NAS message encoding/decoding
- `ngap/`: NGAP message encoding/decoding (ASN.1)
- `pfcp/`: PFCP protocol stack (shared with UPF and SMF)
- `utils/`: General-purpose C++ utilities
- `3gpp/`: 3GPP specification-derived type definitions

---

### 18. `oai-cn5g-fed` — Federation Repository

| Field | Value |
|---|---|
| **GitLab URL** | https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed |
| **Clone** | `git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed.git` |
| **Branch** | `develop` |
| **Commit** | `6b44e2bef7e161d03a5b2c9309db5fa7fe3ba9dd` (2026-03-12) |
| **Language** | YAML / Shell / Python |
| **License** | OAI Public License V1.1 |

**Purpose:**  
The **central integration and deployment repository** for the entire OAI 5GC.
It does not contain NF source code but serves as:

- The primary repo for **Continuous Integration** orchestration across all NFs
- **Docker Compose** deployment recipes for running the full 5GC stack
- **Tutorial and documentation** hub for end-to-end deployments
- **Component registration**: tracks which tag/version of each NF to deploy
- Feature set documentation aggregating all NF capabilities

**Key contents:**
```
component/         Per-NF version/tag tracking files
docker-compose/    Docker Compose files for various deployment scenarios
docs/              Tutorials: simple deployment, debug environments, etc.
ci-scripts/        CI pipeline scripts (Jenkins/GitLab CI)
scripts/           Helper scripts for deployment
test/              Integration test suites
openshift/         OpenShift deployment YAML
```

**Tutorials available (docs/):**
- `DEPLOY_HOME.md` — Container-based simple deployment
- `DEBUG_5G_CORE.md` — Developer/debug container environment

---

## Quick Clone — All Repositories

```bash
BASE=https://gitlab.eurecom.fr/oai/cn5g

# Control Plane NFs
git clone $BASE/oai-cn5g-amf.git
git clone $BASE/oai-cn5g-ausf.git
git clone $BASE/oai-cn5g-lmf.git
git clone $BASE/oai-cn5g-nef.git
git clone $BASE/oai-cn5g-nrf.git
git clone $BASE/oai-cn5g-nssf.git
git clone $BASE/oai-cn5g-nwdaf.git
git clone $BASE/oai-cn5g-pcf.git
git clone $BASE/oai-cn5g-smf.git
git clone $BASE/oai-cn5g-udm.git
git clone $BASE/oai-cn5g-udr.git

# User Plane NFs
git clone $BASE/oai-cn5g-upf.git
git clone $BASE/oai-cn5g-upf-vpp.git
git clone $BASE/oai-cn5g-upf-sdfabric.git

# Shared Infrastructure
git clone $BASE/oai-cn5g-common-build.git
git clone $BASE/oai-cn5g-common-ci.git
git clone $BASE/oai-cn5g-common-src.git

# Federation / Deployment Orchestration
git clone $BASE/oai-cn5g-fed.git
```

---

## Architecture Overview

```
                       ┌──────────────────────────────────┐
                       │         oai-cn5g-fed              │
                       │  (CI / Docker-Compose / Tutorials)│
                       └──────────────────────────────────┘

  ┌──────────┐  N1/N2  ┌──────┐  N8  ┌──────┐  N35 ┌──────┐
  │   gNB    │────────▶│ AMF  │─────▶│ UDM  │─────▶│ UDR  │
  └──────────┘         └──┬───┘      └──────┘      └──────┘
                          │  N11                      ▲
                       ┌──▼───┐  N7  ┌──────┐   N36 │
                       │ SMF  │─────▶│ PCF  │───────┘
                       └──┬───┘      └──────┘
                    N4    │
                     ┌────▼─────────────┐
                     │       UPF        │  ◀── oai-cn5g-upf
                     │  (or UPF-VPP /   │      oai-cn5g-upf-vpp
                     │   UPF-SDFabric)  │      oai-cn5g-upf-sdfabric
                     └────────┬─────────┘
                              │ N6
                       ┌──────▼──────┐
                       │ Data Network│
                       │ (Internet)  │
                       └─────────────┘

  Supporting NFs (all communicate via NRF-based discovery):
  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐  ┌───────┐  ┌──────┐
  │ NRF  │  │ AUSF │  │ NSSF │  │ NEF  │  │ NWDAF │  │ LMF  │
  └──────┘  └──────┘  └──────┘  └──────┘  └───────┘  └──────┘

  Shared submodules (included in each NF repo):
  ┌────────────────────┐  ┌─────────────────┐  ┌─────────────────┐
  │ oai-cn5g-common-src│  │oai-cn5g-common- │  │oai-cn5g-common- │
  │  (C++ source)      │  │   build (CMake) │  │   ci (scripts)  │
  └────────────────────┘  └─────────────────┘  └─────────────────┘
```

---

## Notes on Repository Maturity

| Status | Repositories |
|---|---|
| **Actively maintained (v2.2.0, Dec 2025)** | AMF, AUSF, LMF, NRF, NSSF, PCF, SMF, UDM, UDR, UPF |
| **Infrastructure (continuously updated)** | common-src, common-build, fed |
| **Older / less active** | NEF (2023), NWDAF (2023), UPF-VPP (2023), UPF-SDFabric (2023) |
