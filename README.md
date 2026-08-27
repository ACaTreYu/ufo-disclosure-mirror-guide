# UFO/UAP Disclosure Mirror Guide

A practical, hands-on status report on **which public mirrors actually work** for downloading the major government UFO/UAP disclosures around the world — and which ones look promising but are broken, paywalled, or geo-blocked.

Compiled while building the [Arcbound Interactive UFO Archive](https://arcboundinteractive.com/ufo-archive.html) (a private mirror page, not publicly linked). All download attempts logged below were made on **2026-05-15**.

---

## TL;DR — quick mirror table

| Country | Source we used | Size pulled | Status |
|---|---|---:|---|
| 🇺🇸 USA — PURSUE Releases 01–05 (war.gov) | [Co-Messi Google Drive](https://drive.google.com/drive/folders/1j-cW20aJ1tGMDag6cTldIKtXMMFdpRKo) for R01; war.gov + DVIDS direct for R02–R05 | 18.0 GB | ✅ |
| 🇧🇷 Brazil — Arquivo Nacional | [IA: `BrazilianUFOFiles`](https://archive.org/details/BrazilianUFOFiles) | 1.9 GB | ⚠️ Partial — subset of an 893-document catalogue; unpack the 5 RARs |
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

**Total: 28.9 GB across 1,138 files / 12 countries** — 946 documents (80,689 pages), 148 videos, 30 images, 14 audio recordings. Counts and byte sizes are measured from the mirrored files on disk; page counts are read from the PDFs themselves rather than taken from upstream summaries. Six of those files are broken at source — see [File integrity](#file-integrity--what-is-actually-broken-in-these-mirrors).

Internet Archive bulk downloads use the [`internetarchive`](https://archive.org/developers/internetarchive/) CLI:

```bash
pip install internetarchive
ia download <COLLECTION_ID> --source=original
```

`--source=original` filters out the IA-generated derivatives (OCR, JP2 zips, metadata XML) and only fetches the original uploads — saves enormous bandwidth and disk.

---

## 🇺🇸 USA — PURSUE Releases 01–05 (May–Aug. 2026)

Released by the US Department of War under the PURSUE program (Presidential Unsealing and Reporting System for UAP Encounters). Five tranches to date, 416 files / 18.0 GB mirrored (224 documents totalling 8,751 pages, 148 videos, 30 images, 14 audio recordings):

| Release | Date | Files | Contributing bodies |
|---|---|---:|---|
| 01 | May 8, 2026 | 162 | FBI, DoW, NASA, NARA, State |
| 02 | May 22, 2026 | 64 | DoW (51 IR videos), NASA (Apollo audio), CIA, ODNI, DoE |
| 03 | June 12, 2026 | 72 | CIA, DoW, FBI, NASA |
| 04 | July 10, 2026 | 40 | DoW, NASA, CIA, DoE, FBI |
| 05 | Aug. 7, 2026 | 41 | DoW, FBI, CIA, State, Executive Office of the President |

Official portal: <https://www.war.gov/UFO/>

### Release metadata — the CSV

The portal is a DataTables front-end over a single CSV that covers **every** release:

```
https://www.war.gov/Portals/1/Interactive/2026/UFO/uap-data.csv?release=5
```

29 columns; the useful ones are Release Date, Title, Type (`PDF`/`IMG`/`VID`/`AUD`), Description Blurb, DVIDS Video ID, Agency, Incident Date, Incident Location, and `PDF | Image Link`. Bump `?release=N` as new tranches land. Documents and images join to files on disk by the basename of `PDF | Image Link`.

Same Akamai caveat as the portal itself — the CSV 403s to curl and to any non-browser fetch, including with a spoofed browser UA and a `Referer`. Pull it from a real browser session on a residential IP.

### Video and audio — resolving DVIDS ids

From Release 02 onward the AV assets are hosted on DVIDS, not war.gov, and download under opaque CloudFront names (`DOD_111887401.mp4`). The CSV gives a DVIDS video id per row, not a filename. To map one to the other, fetch the DVIDS page and read the asset path out of it:

```
https://www.dvidshub.net/video/{dvids_id}
  -> https://d34w7g4gy10iej.cloudfront.net/video/2608/DOD_111887401/DOD_111887401.mp4
```

Two things worth knowing:

- A DVIDS page also links **related** videos, so a naive `DOD_\d+` grep over the HTML returns several ids. Match the canonical asset with a backreference — `/(DOD_\d+)/\1\.mp4` — rather than taking the first hit.
- Cross-check each resolved id against the DVIDS page `<title>`, which carries the war.gov canonical id (`DOW-UAP-PR117, Unresolved UAP Report, Gulf of Oman, 2021`). If the title and the CSV title agree for all rows, the mapping is sound. Rename to the canonical id on disk; the CloudFront names carry no meaning.

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

Brazilian Air Force / Ministry of Defense files including the Operação Prato / Colares (1977–78) materials. Transfers to the Arquivo Nacional have been rolling since 2009 under Ordinance 551/GC3, which puts all military branches under a standing order to hand over UAP material.

**[IA: `BrazilianUFOFiles`](https://archive.org/details/BrazilianUFOFiles)** — multi-item collection (142 zipped sub-items + their metadata + OCR derivatives). Originally pulled 1.5 GB / 846 files with default flags; re-ran with `--source=original` to get the 232 actual source PDFs (1.9 GB) and skip OCR/thumbnail/metadata noise.

**`--source=original` is necessary but not sufficient — check what else is in the item.** Alongside the 231 PDFs, the item ships **five RAR archives** holding 224 page scans that no PDF-only pull will ever surface:

| RAR | Contents | Pages |
|---|---|---:|
| `RELATÓRIO 1 prato01.rar` | Operação Prato mission reports, Part 01 | 151 |
| `RELATÓRIO 2 prato02.rar` | Operação Prato mission reports, Part 02 | 54 |
| `JORNAIS jornais.rar` | archived newspaper clippings | 8 |
| `NPA - 09 - C npa.rar` | NPA-09 confidential reporting procedures + witness questionnaire | 6 |
| `DIRETRIZ ESPECÍFICA 04 89 DE0489.rar` | Diretriz Específica nº 04/89, procedure for airspace control units | 5 |

Four of the five also carry a short Portuguese `.txt` stating what the document is — the most reliable titling metadata in the whole Brazilian set, and it only exists inside the archives.

Each folder is numbered JPG page scans. Assemble with [`img2pdf`](https://pypi.org/project/img2pdf/) rather than an image library: it embeds the original JPEG streams verbatim, so the result is a browsable PDF whose page images are byte-identical to the released scans. Sort pages on the integer in the filename, not lexically — otherwise "página 2" lands after "página 11". The entry names are mojibake (`p?gina`) from the original CP850 RAR headers, so trust only the digits.

**Mind the gap between the collection and the mirror.** The Arquivo Nacional's OVNI collection is catalogued at **893 documents spanning 1952–2023**, and a "~20,000+ pages" figure circulates widely (this guide repeated it in an earlier revision). The IA mirror is a *subset*: all its PDFs are structurally intact and parse cleanly, but even with the RARs recovered they total **4,785 pages**, not 20,000. The shortfall is missing coverage, not corruption — do not treat `BrazilianUFOFiles` as the complete collection. Note also that the item was uploaded **2015-12-27**, so it cannot contain anything transferred since; the catalogue runs to 2023. The 893-document figure is what to measure a full mirror against, and reaching it means going to SIAN directly.

An English-language index to the collection exists at [brazilianufoarchives.com](https://brazilianufoarchives.com/) — 495 case write-ups by Arthur Caria, summarising and translating the strictly-military files (1952–2015) with references back to SIAN. It hosts no files and its translations are offered for personal use with credit, so treat it as a finding aid, not a mirror.

Official source: <http://sian.an.gov.br/sianex/consulta/login.asp>, reference code `BR DFANBSB ARX`. SIAN is session-based and fragile to scrape, which is why IA is the convenient bulk source — and why the mirror is partial.

---

## 🇬🇧 UK — MoD UFO Desk

UK Ministry of Defence UFO records (DEFE 24, 31, etc.) covering 1980–2009, released through The National Archives in 8 tranches (2008–2013). The IA mirror pulled 5.3 GB across 233 PDFs; measured at **40,468 pages**, which is the one widely-quoted figure ("~40,500 pages") that survives contact with the actual files.

- **[IA: `BritishUFOFiles`](https://archive.org/details/BritishUFOFiles)** — primary mirror used.
- Also exists: `UkUfoDocumentsPart1` / `UkUfoDocumentsPart2` (separate IA items, partial overlap).
- Official source: <https://www.nationalarchives.gov.uk/help-with-your-research/research-guides/ufos/>. TNA serves individual files but charges after a free-month window.

**One file in the IA mirror is a truncated fragment.** `defe-31-184-1.pdf` is 5,891,072 bytes, has no `%%EOF`, and no PDF reader will open it (`code=7: Invalid number of pages`). It is not a copy error — the bytes on IA are what they are, dated 2015-12-27. The real record, [DEFE 31/184/1](https://discovery.nationalarchives.gov.uk/details/r/C11705543) ("UFO incidents; with redactions", 1994 Jan 12 – 1994 May 31), is a **240.5 MB** PDF at TNA. The IA copy is roughly 2.4% of it.

Recovering it means going to TNA: the record is £3.50, or **free if you sign in** with a (free) TNA account, then Add to basket → checkout at £0 → download. There is no anonymous direct-download URL for it.

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

## File integrity — what is actually broken in these mirrors

A mirror that downloads without error is not the same as a mirror that *works*. Every PDF across all twelve countries was opened and page-counted; **6 of 946 are unusable**, and every one of them was already broken upstream — none was damaged in transit.

| File | Country | Size on disk | Problem |
|---|---|---:|---|
| `defe-31-184-1.pdf` | 🇬🇧 UK | 5.9 MB | Truncated, no `%%EOF`, will not open. TNA original is 240.5 MB — see the UK section. |
| `ufo-release-01/pdfs/65_hs1-834228961_62-hq-83894_serial_153.pdf` | 🇺🇸 USA | 8 KB | No `%%EOF`, parses to zero pages. This is the FBI "Serial 153" file that the Workers CDN 404'd on; the `BruceLanLan` fallback copy used to recover it is itself a stub. |
| `detection_louange_1.pdf` | 🇫🇷 France | 278 B | Not a PDF — a 278-byte error stub. |
| `detection_louange_5.pdf` | 🇫🇷 France | 278 B | Same. |
| `rapport_Louange_1.pdf` | 🇫🇷 France | 278 B | Same. |
| `stat_poher_71.pdf` | 🇫🇷 France | 278 B | Same. |

The four French files are all exactly 278 bytes, which is the tell: a fixed-size "file" repeated across a collection is a server error page that got saved with a `.pdf` extension. GEIPAN's primary archive at <https://www.cnes-geipan.fr> is the place to re-fetch them.

Worth stating plainly: **every other PDF is structurally sound**, including all 236 Brazilian files. Brazil's shortfall is missing coverage, not corruption.

### Not every file in an item is archival content

`ia download` also brings down `__ia_thumb.jpg` — the preview image the Internet Archive generates for each item's detail page. It is not part of any government release, but it lands in the folder next to the real originals and is easy to index as content. One had done exactly that in each of the nine IA-sourced country folders here, so the archive advertised 39 images when only 30 were released records, and the images view rendered nine tiles of Internet Archive site furniture.

Filter it, along with any `*_meta.xml`, `*_meta.sqlite`, and `*_files.xml` that come with an item — those are IA's own catalogue records, not the government's.

### Verifying a mirror yourself

Two cheap checks catch essentially all of this. Byte-size alone catches none of it — several of these files are large enough to look fine in a directory listing.

```python
import os, fitz  # PyMuPDF

def check(path):
    with open(path, 'rb') as f:                 # 1. structural: is it terminated?
        f.seek(max(0, os.path.getsize(path) - 2048))
        if b'%%EOF' not in f.read():
            return 'TRUNCATED'
    try:                                        # 2. semantic: does it actually parse?
        with fitz.open(path) as d:
            return 'ZERO_PAGES' if d.page_count == 0 else f'ok ({d.page_count}p)'
    except Exception as e:
        return f'UNREADABLE ({e})'
```

Summing the page counts also gives you a real number to compare against whatever page total the collection advertises — which is how the Brazil discrepancy above surfaced.

---

## Bigger lessons — evaluating any "mirror" repo

1. **Read the README before cloning.** Watch for "LFS", "Google Drive", "Railway", "S3", "IPFS", "external", "download script" — that tells you where the binaries actually live.
2. **`du -sh`** the freshly-cloned repo. A "5 GB mirror" that clones to 14 MB is a pointer/scaffolding repo, not the data.
3. **LFS-backed repos on personal GitHub accounts are fragile** — the free LFS quota is 1 GB storage / 1 GB monthly bandwidth, and big public-interest mirrors blow through this in days. Expect 50–80% of LFS-backed mirrors to be quota-exhausted by the time you find them.
4. **Google Drive folders** are the most reliable free hosting for ≤15 GB of binaries — the per-file rate limit is annoying but the Drive web "Download all" zip path sidesteps it.
5. **`ia download --source=original`** is the right default for Internet Archive — saves you from dragging down JP2 zips, OCR derivatives, and metadata XML that you don't want.
6. **Government portals fronted by Akamai/Cloudflare/CloudFront** (war.gov, some .gov.* sites) often refuse cloud-egress IPs. Don't assume curl from your laptop will work from a sandbox. Either pull from a residential IP or find a community mirror. This applies to the portal's *data files* too, not just its HTML — war.gov's `uap-data.csv` 403s to curl even with a browser UA and a `Referer` set.
7. **"Downloaded successfully" is not "usable".** Verify structurally (`%%EOF`) and semantically (does it parse, how many pages) — see [File integrity](#file-integrity--what-is-actually-broken-in-these-mirrors). 6 of our 946 PDFs were dead on arrival, and file size caught none of them.
8. **Measure the collection, then compare it to the advertised total.** Page counts read off the mirrored PDFs are the honest number. Repeating an upstream "~20,000 pages" claim hid the fact that our Brazilian mirror held 4,561 — a coverage gap that looked like a complete mirror until someone counted.
9. **Opaque CDN filenames need a resolution step, and that step needs a cross-check.** DVIDS hands you `DOD_111887401.mp4` with no indication of which record it is. Resolve via the id, then confirm against a second field (the page title vs. the CSV title) before you rename anything.

---

## Provenance

This guide was assembled by attempting each of the above downloads in sequence on 2026-05-15, and revised on 2026-08-27 to cover PURSUE Releases 02–05, to replace the quoted page totals with counts measured from the mirrored files, and to add the [File integrity](#file-integrity--what-is-actually-broken-in-these-mirrors) audit.

Status reflects what worked *that day* — LFS budgets get topped up, mirrors get taken down, Akamai rules change. If you find a mirror that's flipped status, PRs welcome.

Where this guide gives a figure for what a collection *should* contain (Brazil's 893 documents, DEFE 31/184/1's 240.5 MB), that number comes from the holding institution's own catalogue and is cited as such. Every other number here was measured from files on disk.
