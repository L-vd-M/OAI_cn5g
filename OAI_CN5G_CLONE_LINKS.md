# OAI CN5G Repository Clone Links

Local workspace: `/home/lourens/Documents/OAI/Standard/OAI_CN_code-cn5g`

All repos cloned via public HTTPS from `https://gitlab.eurecom.fr/oai/cn5g/`

## Clone Commands

```bash
# Network Functions (Control Plane / Core)
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-amf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-ausf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-lmf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nef.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nrf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nssf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-nwdaf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-pcf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-smf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udm.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-udr.git

# User Plane
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf-vpp.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-upf-sdfabric.git

# Shared Infrastructure
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-build.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-ci.git
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-common-src.git

# Federation / Deployment
git clone https://gitlab.eurecom.fr/oai/cn5g/oai-cn5g-fed.git
```

## Notes
- No Eurecom account required for public HTTPS cloning
- SSH (`git@gitlab.eurecom.fr`) requires a Eurecom account
- Most NF repos use `master` as default branch; shared repos use `develop`
- Latest release across NFs: **v2.2.0** (Dec 2025)
- License: OAI Public License V1.1 (except upf-vpp and upf-sdfabric: Apache 2.0)
