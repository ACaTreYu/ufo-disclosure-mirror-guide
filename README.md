# UFO/UAP Disclosure Mirror Guide

A practical status report on **which public mirrors actually work** for downloading the May 2026 US PURSUE UAP release and the Brazilian Arquivo Nacional UFO files — and which ones look promising but are broken, paywalled, or blocked.

Compiled from hands-on download attempts on 2026-05-15.

---

## TL;DR — what to use

| Want | Use | Size | Status |
|---|---|---|---|
| Full US PURSUE binaries (PDFs + videos + images) | [Co-Messi Google Drive](https://drive.google.com/drive/folders/1j-cW20aJ1tGMDag6cTldIKtXMMFdpRKo) | ~5 GB | ✅ Works, public, no sign-in |
| OCR'd markdown of all US PURSUE PDFs (4,185 pages) | `git clone https://github.com/DenisSergeevitch/UFO-USA` | ~21 MB | ✅ Works, full text |
| US PURSUE inventory + curated stories | `git clone https://github.com/pursue-uap-project/pursueproject` | ~46 MB | ✅ Works, but only 63 of 162 entries |
| US PURSUE 162-item full inventory metadata | `git clone https://github.com/wretcher207/the-ufo-files` | ~10 MB | ✅ Works, metadata only |
| Brazilian UFO files (231 PDFs back to 1968) | `ia download BrazilianUFOFiles` | ~? GB | ✅ Works via `internetarchive` CLI |
| Brazilian official source (Arquivo Nacional SIAN) | http://sian.an.gov.br | — | ⚠️ Session-based queries, fragile to scrape |

---

## US PURSUE Release 01 — May 8, 2026

162 declassified files (120 PDFs, 28 videos, 14 images) totaling ~2.3 GiB on disk, ~5 GB if you also want the music-overlaid video variants. Released by the US Department of War under the PURSUE program (Presidential Unsealing and Reporting System for UAP Encounters).

Official portal: <https://www.war.gov/UFO/>

### ✅ What worked

**[Co-Messi/uap-pursue-release-01](https://github.com/Co-Messi/uap-pursue-release-01)** — pointer repo to a public Google Drive folder.

```bash
pip install gdown
gdown --folder "https://drive.google.com/drive/folders/1j-cW20aJ1tGMDag6cTldIKtXMMFdpRKo"
```

Contains: PDFs (~2.3 GB), images (~15 MB), original videos (~1.2 GB), videos-with-music (~1.3 GB). Total ~5 GB. No sign-in needed.

**[DenisSergeevitch/UFO-USA](https://github.com/DenisSergeevitch/UFO-USA)** — full OCR'd markdown of all 4,185 PDF pages (Gemini-converted). Best for analysis/search if you don't need original binaries. Also ships with `metadata/uap-csv.csv` and `metadata/pdf_manifest.tsv` containing all 162 source URLs.

```bash
git clone https://github.com/DenisSergeevitch/UFO-USA
```

**[pursue-uap-project/pursueproject](https://github.com/pursue-uap-project/pursueproject)** — searchable bilingual web app frontend. `stories.json` contains 63 curated stories with war.gov URLs and rich metadata in EN+ES.

**[wretcher207/the-ufo-files](https://github.com/wretcher207/the-ufo-files)** — `pursue-release-01/full-inventory.md` has the canonical 162-item inventory with case notes, agency breakdown, and substantive-vs-filler analysis.

### ❌ What didn't work

**[ckpxgfnksd-max/uap-release-01](https://github.com/ckpxgfnksd-max/uap-release-01)** — LFS-backed mirror. Repo is intact, but every binary is a Git LFS pointer and the LFS endpoint returns:

> Smudge error: This repository exceeded its LFS budget. The account responsible for the budget should increase it to restore access.

Clone succeeds but `git lfs pull` fails on first object. Working tree is wiped by the failed smudge. **Skip.**

**Direct downloads from `www.war.gov`** — Akamai-fronted, IP-level blocked from at least some non-US/cloud egress points. Tried:

- `curl` (plain) → `403 Forbidden`
- `curl -A "Mozilla/5.0 ..."` (browser UA) → `403`
- `curl -A "..." -H "Referer: https://www.war.gov/UFO/" -H "Accept: ..."` → `403`
- PowerShell `Invoke-WebRequest` (different TLS stack) → `403`
- Even the landing page `/UFO/` → `403`

The block isn't header-based — it's IP/TLS-fingerprint-based at Akamai. From a residential US IP in a real browser it works fine; from cloud/CI/VPN egress, expect 403s.

**[zexiro/uap-disclosure-archive](https://github.com/zexiro/uap-disclosure-archive)** — searchable mirror with full-text OCR, but the 5.6 GB of binaries lives on a Railway persistent volume, not in Git. The repo (~14 MB) is the code/scaffolding only.

**[vfp2/pursue-ufo-files](https://github.com/vfp2/pursue-ufo-files)** — npm-based downloader that drives headless Chromium against war.gov/UFO/. Inherits the same Akamai problem from any IP war.gov doesn't like.

**[ckpxgfnksd-max/uap-release-analyzer](https://github.com/ckpxgfnksd-max/uap-release-analyzer)** — Claude skill that analyzes a local PURSUE corpus into a structured report. Tool, not data. Pair with one of the working mirrors above.

### Recommended fetch sequence

```bash
mkdir ufo-us && cd ufo-us

# 1. Get the binaries (~5 GB, slow)
pip install gdown
gdown --folder "https://drive.google.com/drive/folders/1j-cW20aJ1tGMDag6cTldIKtXMMFdpRKo"

# 2. Get the OCR'd markdown corpus (fast, ~21 MB)
git clone https://github.com/DenisSergeevitch/UFO-USA

# 3. Get curated metadata + per-case notes
git clone https://github.com/wretcher207/the-ufo-files
git clone https://github.com/pursue-uap-project/pursueproject
```

---

## Brazilian UFO Disclosure Files

Brazil has been declassifying UFO records since 2009 under Ordinance 551/GC3. The Arquivo Nacional now holds ~20,000+ pages and reports on ~893 sightings spanning 1952–2015, including the Operação Prato (Colares, 1977–78) materials.

### ✅ What worked

**[Internet Archive: BrazilianUFOFiles](https://archive.org/details/BrazilianUFOFiles)** — 231 PDF files dating back to 1968.

```bash
pip install internetarchive
ia download BrazilianUFOFiles
```

Pulls the full collection. No auth needed.

### ⚠️ Other sources (not attempted in this run)

**Arquivo Nacional SIAN** — <http://sian.an.gov.br/sianex/consulta/login.asp> — official source, reference code `BR DFANBSB ARX`. Session-based UI, fragile to scrape. Use as canonical citation source, not as a bulk-download target.

**[brazilianufoarchives.com](https://brazilianufoarchives.com/acess-all-posts/)** — civilian-curated translations and case summaries. Useful for English-language navigation; not a bulk binary source.

### No GitHub mirror exists (as of 2026-05-15)

Unlike the US release, the Brazilian files do **not** have a dedicated GitHub mirror that I could find. If you want one, the Internet Archive bundle above is the easiest base — pull it locally and push to GitHub Releases or LFS.

---

## Bigger lesson: how to choose a mirror

When evaluating any "mirror" repo for a large declassified release, in order:

1. **Read the README before cloning.** Look for the words "LFS", "Google Drive", "Railway", "S3", "IPFS", "external", "download script". This tells you where the binaries actually live.
2. **`du -sh`** the freshly-cloned repo. A "5 GB mirror" that clones to 14 MB is a pointer/scaffolding repo, not the data.
3. **LFS-backed repos** on personal accounts are fragile — GitHub LFS has a free monthly bandwidth quota (1 GB) and total storage quota (1 GB on free, 5 GB per data pack). Big public-interest mirrors blow through this in days.
4. **Google Drive folders** are the most reliable free hosting for ≤15 GB of binaries — no quotas as long as the owner has space, and `gdown --folder` Just Works.
5. **Original government portals** behind Akamai/Cloudflare/CloudFront are often unreachable from cloud egress IPs. Don't assume a `curl` from your laptop will work from a sandbox.

---

## Provenance

This guide was assembled by attempting each of the above downloads in sequence on 2026-05-15. Status reflects what worked *that day* — LFS budgets get topped up, mirrors get taken down, Akamai rules change. If you find a mirror that's flipped status, PRs welcome.
