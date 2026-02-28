# RVTools → Excalidraw

Convert [RVTools](https://www.robware.net/rvtools/) `.xlsx` exports into ready-to-open [Excalidraw](https://excalidraw.com) infrastructure diagrams — directly in your browser, no server required.

**Live app:** https://floriancasse.github.io/RvToolsToExcalidraw/

---

## What it does

Upload one or more RVTools exports and get a colour-coded diagram showing your VMware infrastructure: sites, clusters, and hosts — each with model, ESXi version, service tag, VM count, CPU and memory utilisation.

Multiple sites are laid out side by side, each assigned a distinct colour from the ITQ palette.

![Diagram example showing site zones with clusters and host boxes]()

---

## Usage

1. Open the [live app](https://floriancasse.github.io/RvToolsToExcalidraw/)
2. Drop one or more RVTools `.xlsx` files onto the upload area
3. Optionally rename each site
4. Click **Generate Excalidraw Diagram**
5. Open the downloaded `.excalidraw` file at [excalidraw.com](https://excalidraw.com)

---

## What's parsed

| RVTools sheet | Data extracted |
|---|---|
| `vHost` | Hostname, cluster, model, ESXi version, service tag, VM count, CPU %, memory % |
| `vSource` | vCenter version |

---

## Running locally (optional)

A Flask version of the app (`app.py`) is included if you prefer a local server.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```

Then open http://localhost:5000.

---

## Tech stack

- Vanilla HTML/CSS/JS (static, no build step)
- [SheetJS](https://sheetjs.com/) for `.xlsx` parsing in the browser
- [Excalidraw](https://excalidraw.com) for diagram rendering
- GitHub Pages for hosting

---

Built with ♥ by Florian Casse
