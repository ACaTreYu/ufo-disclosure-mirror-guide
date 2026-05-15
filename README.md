# UFO/UAP Disclosure Mirror Guide

A practical, hands-on status report on **which public mirrors actually work** for downloading the major government UFO/UAP disclosures around the world — and which ones look promising but are broken, paywalled, or geo-blocked.

Compiled while building the [Arcbound Interactive UFO Archive](https://arcboundinteractive.com/ufo-archive.html) (a private mirror page, not publicly linked). All download attempts logged below were made on **2026-05-15**.

---

## TL;DR — quick mirror table

| Country | Source we used | Size pulled | Status |
|---|---|---:|---|
| 🇺🇸 USA — PURSUE Release 01 (war.gov) | [Co-Messi Google Drive](https://drive.google.com/drive/folders/1j-cW20aJ1tGMDag6cTldIKtXMMFdpRKo) | ~5 GB | ✅ |
| 🇧🇷 Brazil — Arquivo Nacional | [IA: `BrazilianUFOFiles`](https://archive.org/details/BrazilianUFOFiles) | ~1.9 GB | ✅ |
| 🇬🇧 UK — MoD UFO Desk (DEFE 24, 31, etc.) | [IA: `BritishUFOFiles`](https://archive.org/details/BritishUFOFiles) | ~5.3 GB | ✅ |
| 🇨🇦 Canada — Library & Archives Canada | [IA: `CanadaUFO`](https://archive.org/details/CanadaUFO) | ~1.0 GB | ✅ |
| 🇪🇸 Spain — Ejército del Aire | [IA: `SpanishUFOFiles`](https://archive.org/details/SpanishUFOFiles) | ~788 MB | ✅ |
| 🇦🇺 Australia — RAAF / NAA | [IA: `AustralianUFOFiles`](https://archive.org/details/AustralianUFOFiles) | ~1.7 GB | ✅ |
| 🇫🇷 France — GEIPAN | [IA: `FrenchUFOFiles`](https://archive.org/details/FrenchUFOFiles) | ~75 MB | ✅ (partial — GEIPAN's primary archive lives at <https://www.cnes-geipan.fr>) |
| 🇳🇿 New Zealand — NZDF | [IA: `NewZealandUFO`](https://archive.org/details/NewZealandUFO) | ~83 MB | ✅ |
| 🇦🇷 Argentina — CEFAe / CIAE | direct from <https://www.argentina.gob.ar/sites/default/files/> | ~76 MB | ✅ (curated 10 annual reports 2015–2024) |
| 🇩🇰 Denmark — Forsvarets Efterretningstjeneste | [IA: `DanishUFOFiles`](https://archive.org/details/DanishUFOFiles) | ~30 MB | ✅ |
| 🇮🇪 Ireland — Defence Forces FOIA 2007 | [IA: `IrishUFOFiles`](https://archive.org/details/IrishUFOFiles) | ~15 MB | ✅ |
| 🇮🇹 Italy — Cabinet RS/33 press review | [IA: `mujssolinis-ufo-files-italian-press-review`](https://archive.org/details/mujssolinis-ufo-files-italian-press-review) | ~25 MB | ⚠️ Third-party press review, not direct government release |

**Total: ~15.7 GB across 924 files / 12 countries.**

Internet Archive bulk downloads use the [`internetarchive`](https://archive.org/developers/internetarchive/) CLI:

```bash
pip install internetarchive
ia download <COLLECTION_ID> --source=original
```

`--source=original` filters out the IA-generated derivatives (OCR, JP2 zips, metadata XML) and only fetches the original uploads — saves enormous bandwidth and disk.

---

## 🇺🇸 USA — PURSUE Release 01 (May 8, 2026)

162 declassified files (120 PDFs, 28 videos, 14 images), ~2.3 GiB on disk. Released by the US Department of War under the PURSUE program (Presidential Unsealing and Reporting System for UAP Encounters).

Official portal: <https://www.war.gov/UFO/>

### ✅ What worked

**[Co-Messi/uap-pursue-release-01](https://github.com/Co-Messi/uap-pursue-release-01)** — pointer repo to a public Google Drive folder containing PDFs (~2.3 GB), images (~15 MB), original videos (~1.2 GB), and music-overlay videos (~1.3 GB). Total ~5 GB.

```bash
pip install gdown
gdown --folder "https://drive.google.com/drive/folders/1j-cW20aJ1tGMDag6cTldIKtXMMFdpRKo"
```

**Caveat — Google Drive rate limit.** Heavy public folders hit Drive's "too many accesses" / "Cannot retrieve the public link" error mid-way (~1.8 GB in for me). Workarounds:

- Re-run with `--continue`: `gdown --folder --continue "<url>"` (skips files already on disk).
- Or use the Drive web UI's "Download all" — gives you a 3-part ~5 GB zip set that sidesteps the API rate limit entirely.
- Or wait 24h and retry.

**Per-file fallbacks** when individual files are missing from the Drive bundle:

- **Cloudflare Workers CDN** (`https://ufofiles.sales-d08.workers.dev/release_1/<filename>.pdf`) — fronts showmeufos.com. Has most but not all (404s on `65_hs1-...serial_153.pdf` and `dow-uap-d20-mission-report-southern-united-states-2020.pdf`).
- **[BruceLanLan/uap-declassified-2026](https://github.com/BruceLanLan/uap-declassified-2026)** — PDFs committed directly to the repo (no LFS, no CDN). Fetchable via `raw.githubusercontent.com/BruceLanLan/uap-declassified-2026/raw/master/files/<filename>.pdf`. Used to recover the two files the Workers CDN was missing.

**Metadata** (titles, agency, incident dates, blurbs): **[DenisSergeevitch/UFO-USA](https://github.com/DenisSergeevitch/UFO-USA)** — markdown OCR of all 4,185 PDF pages plus `metadata/uap-csv.csv` and `metadata/pdf_manifest.tsv`.

### ❌ What didn't work

- **[ckpxgfnksd-max/uap-release-01](https://github.com/ckpxgfnksd-max/uap-release-01)** — LFS budget exhausted; clone works but `git lfs pull` fails on first object.
- **Direct fetch from `www.war.gov`** — Akamai-fronted, IP-level blocked from at least some non-US/cloud egress points. Plain curl, browser-UA-spoofed curl, and PowerShell `Invoke-WebRequest` (different TLS stack) all 403'd. Even the landing page. From a residential US IP in a real browser it works fine.
- **[zexiro/uap-disclosure-archive](https://github.com/zexiro/uap-disclosure-archive)** — binaries live on Railway, not Git.
- **[vfp2/pursue-ufo-files](https://github.com/vfp2/pursue-ufo-files)** — npm-based scraper that drives headless Chromium against war.gov. Inherits the same Akamai problem.

---

## 🇧🇷 Brazil — Arquivo Nacional

Brazilian Air Force / Ministry of Defense files including the Operação Prato / Colares (1977–78) materials. ~20,000+ pages on ~893 sightings, declassifications rolling since 2009.

**[IA: `BrazilianUFOFiles`](https://archive.org/details/BrazilianUFOFiles)** — multi-item collection (142 zipped sub-items + their metadata + OCR derivatives). Originally pulled 1.5 GB / 846 files with default flags; re-ran with `--source=original` to get the 232 actual source PDFs (~1.9 GB) and skip OCR/thumbnail/metadata noise.

Official source: <http://sian.an.gov.br/sianex/consulta/login.asp>, reference code `BR DFANBSB ARX`. SIAN is session-based and fragile to scrape — use IA as the bulk source.

---

## 🇬🇧 UK — MoD UFO Desk

UK Ministry of Defence UFO records (DEFE 24, 31, etc.) covering 1980–2009, released through The National Archives in 8 tranches (2008–2013). ~40,500 pages claimed; the IA mirror pulled 5.3 GB across 233 PDFs.

- **[IA: `BritishUFOFiles`](https://archive.org/details/BritishUFOFiles)** — primary mirror used.
- Also exists: `UkUfoDocumentsPart1` / `UkUfoDocumentsPart2` (separate IA items, partial overlap).
- Official source: <https://www.nationalarchives.gov.uk/help-with-your-research/research-guides/ufos/>. TNA serves individual files but charges after a free-month window.

---

## 🇨🇦 Canada — Library & Archives Canada

~8,000 pages of declassified Canadian UFO documents.

- **[IA: `CanadaUFO`](https://archive.org/details/CanadaUFO)** — single-collection bulk; 31 PDFs, ~1.0 GB.

---

## 🇪🇸 Spain — Ejército del Aire

Spanish Air Force UFO sighting reports 1962–1995, declassified 1991–1997. Originally digitised for the Ministry of Defence Virtual Library.

- **[IA: `SpanishUFOFiles`](https://archive.org/details/SpanishUFOFiles)** — 81 PDFs, ~788 MB.

---

## 🇦🇺 Australia — RAAF / National Archives of Australia

Old RAAF "Blue Book"-era files transferred to the NAA.

- **[IA: `AustralianUFOFiles`](https://archive.org/details/AustralianUFOFiles)** — 75 PDFs, ~1.7 GB.

---

## 🇫🇷 France — GEIPAN (CNES)

GEIPAN — Groupe d'études et d'informations sur les phénomènes aérospatiaux non identifiés — has been part of CNES (the French space agency) since 1977 and runs a public archive of investigation files since 2007 (~3,200 cases).

- **[IA: `FrenchUFOFiles`](https://archive.org/details/FrenchUFOFiles)** — ~75 MB of selected French UFO files mirrored to IA. This is *not* the full GEIPAN archive.
- Primary source for the full archive: <https://www.cnes-geipan.fr/en/home>. Self-serve searchable database but no single bulk download; cases are individual record pages with attached PDFs. Scraping the per-case endpoints is doable but was out of scope for this round.

---

## 🇳🇿 New Zealand — NZDF

NZDF released ~2,000 pages of UFO files in 2010.

- **[IA: `NewZealandUFO`](https://archive.org/details/NewZealandUFO)** — 9 PDFs, ~83 MB.

---

## 🇦🇷 Argentina — CEFAe / CIAE

The Fuerza Aérea Argentina runs the Centro de Identificación Aeroespacial (CIAE, formerly CEFAe), which publishes an annual "Informe de Resolución de Casos" PDF on the official government portal.

**No IA bulk** — direct from <https://www.argentina.gob.ar/sites/default/files/>. URL pattern shifts across years; the script that worked:

```bash
for y in 2015 2016 2017 2018 2019 2020 2021 2022 2023 2024; do
  for url in \
    "https://www.argentina.gob.ar/sites/default/files/informe_cefae_${y}.pdf" \
    "https://www.argentina.gob.ar/sites/default/files/informe_ciae_${y}.pdf" \
    "https://www.argentina.gob.ar/sites/default/files/2018/11/informe-ciae-${y}.pdf" \
    "https://www.argentina.gob.ar/sites/default/files/2018/11/informe_ciae_${y}.pdf"; do
    code=$(curl -sL -o "informe_${y}.pdf.tmp" -w "%{http_code}" "$url")
    if [ "$code" = "200" ] && head -c 4 "informe_${y}.pdf.tmp" | grep -q "^%PDF"; then
      mv "informe_${y}.pdf.tmp" "informe_${y}.pdf"; break
    fi
  done
  rm -f "informe_${y}.pdf.tmp"
done
```

Got 9 of 10 (2019 was on a URL pattern I couldn't find). Plus a 2023 supplementary PDF ("Origen de más de 350 avistajes de ovnis") under `/sites/default/files/2023/10/`.

Hub page: <https://www.argentina.gob.ar/fuerzaaerea/centro-de-identificacion-aeroespacial>

---

## 🇩🇰 Denmark — Forsvarets Efterretningstjeneste

Danish Defence Intelligence released its UFO files in 2009 — a single consolidated PDF.

- **[IA: `DanishUFOFiles`](https://archive.org/details/DanishUFOFiles)** — 1 PDF, ~30 MB.

---

## 🇮🇪 Ireland — Defence Forces FOIA 2007

Irish Defence Forces released UFO-related records under a 2007 FOIA request.

- **[IA: `IrishUFOFiles`](https://archive.org/details/IrishUFOFiles)** — 1 PDF (Irish Defence Forces FOIA 2007), ~15 MB.

---

## 🇮🇹 Italy — Cabinet RS/33 (Mussolini-era)

Italy's pre-WWII Cabinet RS/33 records on UFO-like phenomena. ⚠️ The IA collection is a **press review compiled by researcher Alfredo Lissoni**, not a direct release from the Italian Air Force — included for completeness but it's third-party.

- **[IA: `mujssolinis-ufo-files-italian-press-review`](https://archive.org/details/mujssolinis-ufo-files-italian-press-review)** — 1 PDF, ~25 MB.

---

## Countries we skipped (no public bulk archive)

These governments have *some* UFO records in the public record (FOIA returns, court testimony, single press releases), but no canonical bulk archive worth mirroring as of 2026-05-15:

- 🇨🇱 **Chile (CEFAA)** — case-by-case access only, formal DGAC request required.
- 🇲🇽 **Mexico (Sedena)** — the 2004 IR-pod video circulates widely but has no canonical .gob.mx source page (the file was released to Jaime Maussan personally, not as a portal).
- 🇷🇺 **Russia / former Soviet** — partial scans of the "Setka" archive exist in private hands; no official portal.
- 🇧🇪 **Belgium (1989–90 wave)** — research-community materials (SOBEPS), not a government archive.
- 🇯🇵 **Japan**, 🇵🇪 **Peru**, 🇪🇨 **Ecuador**, 🇷🇴 **Romania** — military reports exist but no public archive.

If you find a bulk source for any of these, open an issue / PR.

---

## Bigger lessons — evaluating any "mirror" repo

1. **Read the README before cloning.** Watch for "LFS", "Google Drive", "Railway", "S3", "IPFS", "external", "download script" — that tells you where the binaries actually live.
2. **`du -sh`** the freshly-cloned repo. A "5 GB mirror" that clones to 14 MB is a pointer/scaffolding repo, not the data.
3. **LFS-backed repos on personal GitHub accounts are fragile** — the free LFS quota is 1 GB storage / 1 GB monthly bandwidth, and big public-interest mirrors blow through this in days. Expect 50–80% of LFS-backed mirrors to be quota-exhausted by the time you find them.
4. **Google Drive folders** are the most reliable free hosting for ≤15 GB of binaries — the per-file rate limit is annoying but the Drive web "Download all" zip path sidesteps it.
5. **`ia download --source=original`** is the right default for Internet Archive — saves you from dragging down JP2 zips, OCR derivatives, and metadata XML that you don't want.
6. **Government portals fronted by Akamai/Cloudflare/CloudFront** (war.gov, some .gov.* sites) often refuse cloud-egress IPs. Don't assume curl from your laptop will work from a sandbox. Either pull from a residential IP or find a community mirror.

---

## Provenance

This guide was assembled by attempting each of the above downloads in sequence on 2026-05-15. Status reflects what worked *that day* — LFS budgets get topped up, mirrors get taken down, Akamai rules change. If you find a mirror that's flipped status, PRs welcome.
