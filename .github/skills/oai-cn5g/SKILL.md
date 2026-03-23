---
name: oai-cn5g
description: 'Work with the OpenAirInterface 5G Core Network (OAI CN5G) repositories. Use when cloning, analyzing, building, configuring, or navigating any of the 18 OAI CN5G repositories (AMF, AUSF, LMF, NEF, NRF, NSSF, NWDAF, PCF, SMF, UDM, UDR, UPF, UPF-VPP, UPF-SDFabric, common-build, common-ci, common-src, fed). Use for 5G core network function questions, repository structure navigation, deployment with Docker Compose, and understanding 3GPP NF roles.'
argument-hint: 'e.g. "clone all repos", "explain AMF source structure", "how to build SMF"'
---

# OAI CN5G — Agent Skill

## Overview

This skill covers the **OpenAirInterface 5G Core Network** codebase — 18 Git
repositories implementing 3GPP-compliant 5GC network functions, hosted at:

`https://gitlab.eurecom.fr/oai/cn5g/`

Full repository details: [OAI_CN5G_REPOSITORIES.md](../../OAI_CN5G_REPOSITORIES.md)  
Clone links reference: [OAI_CN5G_CLONE_LINKS.md](../../OAI_CN5G_CLONE_LINKS.md)

---

## Repository Map

| Repo | NF | Role |
|---|---|---|
| `oai-cn5g-amf` | AMF | UE registration, mobility, NAS/NGAP |
| `oai-cn5g-ausf` | AUSF | 5G-AKA / EAP-AKA' authentication |
| `oai-cn5g-lmf` | LMF | UE positioning and location services |
| `oai-cn5g-nef` | NEF | External AF exposure, northbound API |
| `oai-cn5g-nrf` | NRF | NF registry, discovery, OAuth2 tokens |
| `oai-cn5g-nssf` | NSSF | Network slice selection |
| `oai-cn5g-nwdaf` | NWDAF | Data analytics microservices (Python) |
| `oai-cn5g-pcf` | PCF | AM/SM policy control |
| `oai-cn5g-smf` | SMF | PDU session management, PFCP to UPF |
| `oai-cn5g-udm` | UDM | Subscriber data management, AV generation |
| `oai-cn5g-udr` | UDR | Data storage backend (MySQL) |
| `oai-cn5g-upf` | UPF | User-plane forwarding (GTPv1-U / PFCP) |
| `oai-cn5g-upf-vpp` | UPF-VPP | High-perf UPF (VPP/DPDK-based, Apache 2.0) |
| `oai-cn5g-upf-sdfabric` | UPF-SD | SD-Fabric UPF integration (Apache 2.0) |
| `oai-cn5g-common-build` | — | Shared CMake + install scripts (submodule) |
| `oai-cn5g-common-ci` | — | Shared CI scripts (submodule) |
| `oai-cn5g-common-src` | — | Shared C++ source (submodule) |
| `oai-cn5g-fed` | — | Federation: Docker-Compose, CI, tutorials |

---

## Key Facts

- **Current release:** v2.2.0 (December 2025)
- **Language:** C++ (all NFs except NWDAF which is Python microservices)
- **License:** OAI Public License V1.1 (UPF-VPP, UPF-SDFabric: Apache 2.0)
- **Tested OS:** Ubuntu 20.04 / 22.04, RHEL9 / Rocky9
- **Branch convention:** NF repos → `master`; shared repos → `develop`
- **No Eurecom account needed** for public HTTPS cloning

---

## Common Procedures

### Clone All Repositories

```bash
BASE=https://gitlab.eurecom.fr/oai/cn5g
for repo in oai-cn5g-amf oai-cn5g-ausf oai-cn5g-lmf oai-cn5g-nef \
            oai-cn5g-nrf oai-cn5g-nssf oai-cn5g-nwdaf oai-cn5g-pcf \
            oai-cn5g-smf oai-cn5g-udm oai-cn5g-udr oai-cn5g-upf \
            oai-cn5g-upf-vpp oai-cn5g-upf-sdfabric oai-cn5g-common-build \
            oai-cn5g-common-ci oai-cn5g-common-src oai-cn5g-fed; do
  git clone $BASE/$repo.git
done
```

### Check Versions of All Cloned Repos

```bash
python3 -c "
import subprocess, os
repos = [d for d in os.listdir('.') if os.path.isdir(d+'/.git')]
for r in sorted(repos):
    b = subprocess.check_output(['git','-C',r,'branch','--show-current'],text=True).strip()
    c = subprocess.check_output(['git','-C',r,'log','-1','--format=%h %ai %s'],text=True).strip()
    print(f'{r:35s} [{b}] {c}')
"
```

### Understand NF Source Structure (C++ NFs)

Every C++ NF follows this layout:
```
src/
├── <nf>_app/       Core application logic and state machines
├── api-server/     SBI REST API (auto-generated from 3GPP YAML, using pistache)
├── common/         NF-specific utilities and headers
├── common-src/     Symlink → oai-cn5g-common-src submodule
└── oai_<nf>/       Main entry point and root CMakeLists.txt
```

Additional NF-specific directories (SMF/UPF):
- `pfcp/` — PFCP protocol stack
- `gtpv1u/` — GTPv1-U tunnel implementation
- `itti/` — Inter-task interface (message passing)

### Find the Configuration File

Each NF has a YAML config:
```bash
ls <repo>/etc/config.yaml    # primary config
ls <repo>/etc/amf.conf       # legacy conf (AMF only)
```

### Build a Network Function

```bash
cd <repo>
./build/scripts/build_amf --install-deps    # first time: install dependencies
./build/scripts/build_amf --clean           # clean build
./build/scripts/build_amf                   # incremental build
```
> Replace `amf` with the NF name (`ausf`, `smf`, `nrf`, etc.)

### Deploy with Docker Compose (via oai-cn5g-fed)

```bash
cd oai-cn5g-fed/docker-compose
# Edit conf/ files as needed, then:
docker compose -f docker-compose-basic-nrf.yaml up -d
```

See `oai-cn5g-fed/docs/DEPLOY_HOME.md` for the full tutorial.

---

## Shared Submodule Pattern

Each NF repo includes three Git submodules:

| Submodule path | Points to |
|---|---|
| `build/common-build` | `oai-cn5g-common-build` |
| `ci-scripts/common` | `oai-cn5g-common-ci` |
| `src/common-src` | `oai-cn5g-common-src` |

After cloning a single NF, initialise submodules with:
```bash
git submodule update --init --recursive
```

---

## 3GPP Interface Reference

| Interface | Between | Protocol |
|---|---|---|
| N1 | UE ↔ AMF | NAS (5G) |
| N2 | gNB ↔ AMF | NGAP (SCTP) |
| N3 | gNB ↔ UPF | GTPv1-U (UDP) |
| N4 | SMF ↔ UPF | PFCP (UDP) |
| N6 | UPF ↔ DN | IP |
| N7 | SMF ↔ PCF | HTTP/2 REST |
| N8 | AMF ↔ UDM | HTTP/2 REST |
| N11 | AMF ↔ SMF | HTTP/2 REST |
| N12 | AMF ↔ AUSF | HTTP/2 REST |
| N27 | NRF ↔ NRF | HTTP/2 REST |
| Nnrf | Any NF ↔ NRF | HTTP/2 REST |
