# IRFlow Timeline

![IRFlow Timeline home screen — capability launcher with Process Inspector, Lateral Movement, Persistence, Sigma, Collect AI Artifacts, Master File Table, USN Journal, and Open & Explore](assets/IRFlow-Timeline-Home.png)

Desktop forensic timeline analysis for macOS and Linux. Import, search, and investigate EVTX, CSV, XLSX, Plaso, `$MFT`, `$J`, and local AI assistant artifacts — with **AI Secret Hunt** and the analytics DFIR professionals actually need. Built on Electron + SQLite to handle millions of rows without breaking a sweat.

Inspired by Eric Zimmerman's Timeline Explorer for Windows.

### Key Features

- **AI Artifacts** — Collect local AI history from Claude Code, Codex, Grok Build, ChatGPT Desktop, Gemini CLI, Cursor, Copilot, Windsurf, and Continue into one timeline tab; **ChatGPT Computer History** for macOS interaction telemetry (macOS only); **AI Secret Hunt** for exposed keys, tokens, and credentials
- **Raw NTFS Artifact Import** — Direct ingestion of `$MFT` and `$UsnJrnl` (`$J`) with full path reconstruction, SI/FN timestamps, and change reason mapping
- **Ransomware Analytics** — Automated impact analysis from `$MFT` data: bulk rename detection, entropy-based extension analysis, ransom note identification, and temporal clustering
- **VirusTotal Enrichment** — IOC matching with bulk VT lookups, malware family extraction, verdict badges, relationship pivoting, and local caching
- **Process Inspector** — Parent-child process tree analysis with 340+ MITRE ATT&CK detection rules
- **Lateral Movement Tracker** — Network logon and RDP session visualization as interactive force-directed graphs
- **RDP Bitmap Cache Recovery** — Recover `bcache*.bmc` and `cache????.bin` artifacts with bundled bmc-tools, preview images, and export evidence packages
- **Persistence Analyzer** — 30+ persistence techniques with account chain detection, cross-technique correlation, and PowerShell 4104 script block reassembly
- **IOC Matching** — 17+ indicator categories with auto-defanging, inline highlighting, CSV/HTML export with VT enrichment data

For the full feature list and documentation, visit the **[IRFlow Timeline Docs](https://r3nzsec.github.io/irflow-timeline/)**.

## Download

Download prebuilt Linux packages from [GitHub Releases](../../releases). Linux release assets target x86_64 and include an AppImage and a Debian package. Ubuntu 24.04 is the verified Linux environment. AppImage launch may require FUSE 2 (`libfuse2t64` on Ubuntu 24.04); use the Debian package if FUSE is unavailable. Older distributions and Linux arm64 are unverified.

```bash
# From the directory containing the downloaded AppImage
chmod +x ./IRFlow-Timeline-*-x86_64.AppImage
./IRFlow-Timeline-*-x86_64.AppImage

# Or install the downloaded Debian package
sudo apt install ./IRFlow-Timeline-*-amd64.deb
```

After the Linux release workflow is merged, pushing a version-matching `v` tag builds and uploads these packages. You can also manually run that workflow for an existing matching tag that contains the Linux build and release workflow. In-app automatic updates are not configured for Linux.

## Building from Source

**Prerequisites (for developers only):**
- Node.js 22.14 or newer and npm
- macOS 12+ with Xcode command-line tools, or Linux x86_64 with build tools, Python 3, curl, and unzip

```bash
git clone https://github.com/r3nzsec/irflow-timeline.git
cd irflow-timeline
npm install
npx electron-rebuild -f -w better-sqlite3

# Development (hot-reload)
npm run dev

# Build + launch
npm run start

# Package as universal DMG
npm run dist:universal
```

Output in `release/`.

### Linux

On Ubuntu 24.04 x86_64, install the build and Electron runtime dependencies before building:

```bash
sudo apt-get update
sudo apt-get install -y build-essential python3 curl unzip git \
  libgtk-3-0 libnss3 libasound2t64 libgbm1 libxss1 libxtst6 libxshmfence1

git clone https://github.com/r3nzsec/irflow-timeline.git
cd irflow-timeline
npm ci
npm run dist:linux
```

`npm run dist:linux` creates AppImage and Debian packages in `release/`. For an unpacked directory useful during development, run `npm run dist:linux:dir`. `build.sh` is a macOS-only helper; use the npm commands above on Linux. Linux arm64 bundling is configured but unverified. These npm commands create local artifacts; in-app automatic updates are not configured.

Launch the AppImage with:

```bash
chmod +x release/*.AppImage
./release/*.AppImage
```

Or install the Debian package with:

```bash
sudo apt install ./release/*.deb
```

## Credits & Acknowledgments

Inspired by [Eric Zimmerman's Timeline Explorer](https://ericzimmerman.github.io/).

### Open Source Projects

| Project | Usage | Link |
|---------|-------|------|
| **Electron** | Application framework | [electron/electron](https://github.com/electron/electron) |
| **better-sqlite3** | High-performance SQLite engine with WAL mode, FTS5 | [WiseLibs/better-sqlite3](https://github.com/WiseLibs/better-sqlite3) |
| **@ts-evtx/core** | Native Windows EVTX event log parsing | [NickSmet/ts-evtx](https://github.com/NickSmet/ts-evtx) |
| **Plaso (log2timeline)** | Forensic timeline generation (we import Plaso SQLite output) | [log2timeline/plaso](https://github.com/log2timeline/plaso) |
| **ExcelJS** | XLSX streaming reader | [exceljs/exceljs](https://github.com/exceljs/exceljs) |
| **SheetJS (xlsx)** | XLSX parsing | [SheetJS/sheetjs](https://github.com/SheetJS/sheetjs) |
| **csv-parser** | CSV/TSV streaming parser | [mafintosh/csv-parser](https://github.com/mafintosh/csv-parser) |
| **React** | UI rendering | [facebook/react](https://github.com/facebook/react) |
| **Vite** | Build tooling and hot-reload | [vitejs/vite](https://github.com/vitejs/vite) |
| **VitePress** | Documentation site | [vuejs/vitepress](https://github.com/vuejs/vitepress) |
| **electron-builder** | macOS DMG packaging | [electron-userland/electron-builder](https://github.com/electron-userland/electron-builder) |
| **bmc-tools** | RDP Bitmap Cache recovery | [ANSSI-FR/bmc-tools](https://github.com/ANSSI-FR/bmc-tools) |

### DFIR Community

- [Eric Zimmerman](https://ericzimmerman.github.io/) -- Timeline Explorer for Windows, the original inspiration for this project
- [log2timeline/Plaso](https://github.com/log2timeline/plaso) -- Super timeline generation framework by Kristinn Gudjonsson and contributors
- [SANS DFIR](https://www.sans.org/digital-forensics-incident-response/) -- DFIR training and community resources
- [The DFIR Report](https://thedfirreport.com/) -- Real-world intrusion analysis reports that informed threat detection patterns
- [CyberCX](https://cybercx.com.au/blog/ntfs-usnjrnl-rewind/) -- NTFS $UsnJrnl research that informed $J parsing implementation

### Beta Testers

Thanks to the following people for testing and providing feedback:

- [Maddy Keller](https://www.linkedin.com/in/madeleinekeller98/)
- [Omar Jbari](https://www.linkedin.com/in/jbariomar/)
- [Nicolas Bareil](https://www.linkedin.com/in/nbareil/)
- [Dominic Rathmann](https://www.linkedin.com/in/dominic-rathmann-77664323b/)
- [Chip Riley](https://www.linkedin.com/in/criley4640/)

## License

Apache-2.0
