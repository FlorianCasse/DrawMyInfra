# DrawMyInfra

Convert VMware infrastructure exports into ready-to-open [Excalidraw](https://excalidraw.com) diagrams — directly in your browser, no server required.

**Live app (production):** https://floriancasse.github.io/DrawMyInfra/
**Dev preview:** https://floriancasse.github.io/DrawMyInfra/dev/

---

## Supported sources

| Tool | Format | Sheets used |
|---|---|---|
| [RVTools](https://www.robware.net/rvtools/) | `.xlsx` | `vHost`, `vSource` |
| [LiveOptics](https://www.liveoptics.com/) | `.xlsx` | `ESX Hosts`, `ESX Performance` |

The app auto-detects the format — just drop any supported file and it works.

---

## What it does

Upload one or more exports (mix and match sources) and get a colour-coded diagram showing your VMware infrastructure: sites, clusters, and hosts — each with model, ESXi version, service tag, VM count, CPU and memory utilisation.

Multiple sites are laid out side by side, each assigned a distinct colour palette.

### VCF 9 Compatibility Check

Tick the **Check VCF 9 Compatibility** checkbox before generating to check each host against the [Broadcom Compatibility Guide](https://compatibilityguide.broadcom.com/) and annotate it with a readiness indicator:

| Indicator | Meaning |
|---|---|
| ✅ VCF 9.0 + 9.1 Ready | Server model supports both ESXi 9.0 and 9.1 |
| ✅ VCF 9.0 Ready | Server model supports ESXi 9.0 only |
| ❌ Not VCF9 Ready | Server model not found in the Compatibility Guide |
| ⚠️ VCF9 ? | Server model could not be determined |

Incompatible hosts are highlighted with a red border in the diagram.

A **VCF 9 Readiness Report** is also generated as a table with per-host compatibility status, summary counts, and downloadable TXT/CSV exports.

### VCF/VVF License Calculator

Tick the **VCF/VVF License Calculator** checkbox and select a deployment type (VCF or VVF) to calculate foundation core licensing from your uploaded data — no need to connect to vCenter or run PowerCLI.

**Formula** (per host):
```
Foundation Cores = Sockets × max(Cores per Socket, 16)
vSAN Entitled TiB = Foundation Cores × TiB per Core
```

| Deployment | TiB per Core |
|---|---|
| VCF | 1 TiB |
| VVF | 0.25 TiB |

The report includes a per-host table with totals and a downloadable CSV. Hosts with missing CPU socket or core data are flagged and excluded from totals.

> **Note:** vSAN add-on licenses are required when actual vSAN capacity exceeds the entitled TiB. This cannot be determined from RVTools/LiveOptics data — verify against your actual vSAN capacity.

Reference: [Broadcom KB 313548](https://knowledge.broadcom.com/external/article?legacyId=313548)

---

## Usage

1. Open the [live app](https://floriancasse.github.io/DrawMyInfra) (or the [dev preview](https://floriancasse.github.io/DrawMyInfra/dev/))
2. Drop one or more `.xlsx` files onto the upload area
3. Optionally rename each site
4. Optionally tick **Check VCF 9 Compatibility** to add Broadcom Compatibility Guide readiness indicators
5. Optionally tick **VCF/VVF License Calculator** and choose VCF or VVF to get a foundation core licensing report
6. Click **Generate Diagram**
7. Preview the diagram live on the page, and review the license report if enabled
8. Click **Download .excalidraw** and open it at [excalidraw.com](https://excalidraw.com)
9. Click **Download CSV** to export the license report

---

## Running locally (optional)

A Flask version (`app.py`) is included for local use.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```

Then open http://localhost:5000.

---

## Tech stack

- Vanilla HTML/CSS/JS — no build step
- [SheetJS](https://sheetjs.com/) for `.xlsx` parsing in the browser
- [@excalidraw/excalidraw](https://www.npmjs.com/package/@excalidraw/excalidraw) for the embedded diagram viewer
- GitHub Pages for hosting
- [Broadcom Compatibility Guide](https://compatibilityguide.broadcom.com/) for VCF 9 compatibility data

---

## Deployment

A GitHub Actions workflow deploys both branches to GitHub Pages:

| Branch | URL |
|---|---|
| `main` | https://floriancasse.github.io/DrawMyInfra/ |
| `dev` | https://floriancasse.github.io/DrawMyInfra/dev/ |

Every push to either branch triggers a rebuild.

---

Built with ♥ by Florian Casse
