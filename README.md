# OAI CN5G — Workspace

This repository is a **meta-workspace** aggregating the 18 OpenAirInterface 5G Core Network repositories as Git submodules.

Each submodule points to the upstream public repository on the [OAI GitLab](https://gitlab.eurecom.fr/oai/cn5g/).

## Quick Start

Clone this workspace with all submodules:

```bash
git clone --recurse-submodules https://github.com/L-vd-M/OAI_cn5g.git
```

Or, after a plain clone:

```bash
git submodule update --init --recursive
```

## Repositories

| Submodule | Network Function | Role |
|---|---|---|
| [oai-cn5g-amf](oai-cn5g-amf/) | AMF | Access & Mobility Management |
| [oai-cn5g-ausf](oai-cn5g-ausf/) | AUSF | Authentication Server |
| [oai-cn5g-lmf](oai-cn5g-lmf/) | LMF | Location Management |
| [oai-cn5g-nef](oai-cn5g-nef/) | NEF | Network Exposure |
| [oai-cn5g-nrf](oai-cn5g-nrf/) | NRF | NF Repository & Discovery |
| [oai-cn5g-nssf](oai-cn5g-nssf/) | NSSF | Network Slice Selection |
| [oai-cn5g-nwdaf](oai-cn5g-nwdaf/) | NWDAF | Network Data Analytics |
| [oai-cn5g-pcf](oai-cn5g-pcf/) | PCF | Policy Control |
| [oai-cn5g-smf](oai-cn5g-smf/) | SMF | Session Management |
| [oai-cn5g-udm](oai-cn5g-udm/) | UDM | Unified Data Management |
| [oai-cn5g-udr](oai-cn5g-udr/) | UDR | Unified Data Repository |
| [oai-cn5g-upf](oai-cn5g-upf/) | UPF | User Plane Function |
| [oai-cn5g-upf-vpp](oai-cn5g-upf-vpp/) | UPF-VPP | User Plane (VPP/DPDK) |
| [oai-cn5g-upf-sdfabric](oai-cn5g-upf-sdfabric/) | UPF-SDFabric | User Plane (SD-Fabric) |
| [oai-cn5g-common-build](oai-cn5g-common-build/) | — | Shared build scripts |
| [oai-cn5g-common-ci](oai-cn5g-common-ci/) | — | Shared CI scripts |
| [oai-cn5g-common-src](oai-cn5g-common-src/) | — | Shared C++ source |
| [oai-cn5g-fed](oai-cn5g-fed/) | — | Federation / Docker-Compose deployment |

## Documentation

- [OAI_CN5G_REPOSITORIES.md](OAI_CN5G_REPOSITORIES.md) — Detailed description of all repositories
- [OAI_CN5G_CLONE_LINKS.md](OAI_CN5G_CLONE_LINKS.md) — Clone commands for all repositories

## License

OAI Public License V1.1 — see individual submodule repositories for details.  
`oai-cn5g-upf-vpp` and `oai-cn5g-upf-sdfabric` are licensed under Apache 2.0.
