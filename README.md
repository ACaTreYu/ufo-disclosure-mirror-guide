# UFO/UAP Disclosure Mirror Guide

A practical, hands-on status report on **which public mirrors actually work** for downloading the major government UFO/UAP disclosures around the world — and which ones look promising but are broken, paywalled, or geo-blocked.

Compiled while building the [Arcbound Interactive Government UAP Disclosure Repository](https://arcboundinteractive.com/contact.html) (reached through the site's "Contact" nav). The Release 01 attempts logged below were made on **2026-05-15**; the **PURSUE Release 02** section was added on **2026-05-25** after the May 22 drop; **Release 03** was mirrored on **2026-06-15**, **Release 04** on **2026-07-16**, and **Release 05** on **2026-08-27** (guide updated 2026-08-29).

---

## TL;DR — quick mirror table

| Country | Source we used | Size pulled | Status |
|---|---|---:|---|
| 🇺🇸 USA — PURSUE Release 01 (war.gov) | [Co-Messi Google Drive](https://drive.google.com/drive/folders/1j-cW20aJ1tGMDag6cTldIKtXMMFdpRKo) | ~5 GB | ✅ |
| 🇺🇸 USA — PURSUE Release 02 (war.gov) | direct from war.gov (headed) + DVIDS — **no public mirror yet** | ~5.4 GB | ⚠️ direct-only |
| 🇺🇸 USA — PURSUE Release 03 (war.gov) | direct from war.gov (headed) + DVIDS — **no public mirror yet** | ~5.5 GB | ⚠️ direct-only |
| 🇺🇸 USA — PURSUE Release 04 (war.gov) | direct from war.gov (headed) + DVIDS — **no public mirror yet** | ~1.7 GB | ⚠️ direct-only |
| 🇺🇸 USA — PURSUE Release 05 (war.gov) | war.gov bulk zips (headed browser) + DVIDS id resolution — **no public mirror** | ~646 MB | ⚠️ direct-only |
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

**Total: ~28.9 GB across 1,138 files / 12 countries** (the original 924 + PURSUE Release 02's 64 / ~5.4 GB + Release 03's 72 / ~5.5 GB + Release 04's 40 / ~1.7 GB + Release 05's 41 / ~646 MB, plus five Brazilian documents reassembled from RAR archives, minus nine `__ia_thumb.jpg` files that are Internet Archive furniture rather than released records — see the Brazil section).

Internet Archive bulk downloads use the [`internetarchive`](https://archive.org/developers/internetarchive/) CLI:

```bash
pip install internetarchive
ia download <COLLECTION_ID> --source=original
```

`--source=original` filters out the IA-generated derivatives (OCR, JP2 zips, metadata XML) and only fetches the original uploads — saves enormous bandwidth and disk.

---

## 🇺🇸 USA — PURSUE (war.gov)

Released by the US Department of War under the PURSUE program (Presidential Unsealing and Reporting System for UAP Encounters). Official portal: <https://www.war.gov/UFO/> — additional files drop on a rolling basis. Five releases so far (416 files mirrored across them).

### Release 01 (May 8, 2026)

162 declassified files (120 PDFs, 28 videos, 14 images), ~2.3 GiB on disk.

#### ✅ What worked

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

#### ❌ What didn't work

- **[ckpxgfnksd-max/uap-release-01](https://github.com/ckpxgfnksd-max/uap-release-01)** — LFS budget exhausted; clone works but `git lfs pull` fails on first object.
- **Direct fetch from `www.war.gov`** — Akamai-fronted, IP-level blocked from at least some non-US/cloud egress points. Plain curl, browser-UA-spoofed curl, and PowerShell `Invoke-WebRequest` (different TLS stack) all 403'd. Even the landing page. From a residential US IP in a real browser it works fine.
- **[zexiro/uap-disclosure-archive](https://github.com/zexiro/uap-disclosure-archive)** — binaries live on Railway, not Git.
- **[vfp2/pursue-ufo-files](https://github.com/vfp2/pursue-ufo-files)** — npm-based scraper that drives headless Chromium against war.gov. Inherits the same Akamai problem.

### Release 02 (May 22, 2026)

The second PURSUE drop. Highlights: the **Lake Huron F-16 shootdown** clip (first released footage of a US military aircraft engaging a UAP), submarine **transmedium** footage of spheres entering/exiting water, 116 pages of Sandia "green orb" Cold War investigations (209 sightings, 1948–1950), and **Apollo 12** crew audio describing "streaks of lights." Spans War Dept., NASA, DOE, CIA, and ODNI.

I pulled **64 files — 51 videos, 7 audio, 6 PDFs — ~5.4 GB on disk.**

Direct page: <https://www.war.gov/UFO/?releaseDate=Release+02>

#### ⚠️ No public bulk mirror yet

Unlike Release 01, there is **no** community Google Drive / GitHub bundle for Release 02 as of 2026-05-25:

- **[Co-Messi/uap-pursue](https://github.com/Co-Messi/uap-pursue)** (the renamed Release-01 repo) still only covers Release 01.
- **[BruceLanLan/uap-declassified-2026](https://github.com/BruceLanLan/uap-declassified-2026)** is Release 01 only.
- **[warufo.com](https://warufo.com/)** and **[ufo-declassified.com](https://ufo-declassified.com/)** are *analysis / search* layers — they front `war.gov/medialink/` links and organize the 222 docs thematically, but don't host the originals for bulk download.

So the only source is war.gov itself.

#### ✅ What worked — pulling direct from war.gov

The Akamai cloud-egress block from Release 01 still applies (plain curl and cloud-IP requests 403, landing page included). What worked was driving a **headed** browser from a residential IP:

- **PDFs + metadata** — headed [Playwright](https://playwright.dev/) against `war.gov/UFO/?releaseDate=Release+02`, scraping the per-file cards for titles, agency, incident date, and blurbs. Headless inherits the same Akamai 403, so run it headed (or capture from a real residential browser session).
- **Videos + audio** — these don't sit on war.gov directly; they stream from **DVIDS** as HLS. Grab the `.m3u8` playlist the player loads and pull the backing **CloudFront** `.mp4` (standard HLS capture, e.g. `ffmpeg -i "<playlist>.m3u8" -c copy out.mp4`, or fetch the CloudFront origin mp4 directly). The audio items are MP4-wrapped too (`NASA-UAP-D0xx.mp4`).

If a public Release 02 mirror surfaces, open a PR — until then, residential-IP + DVIDS is the path.

### Release 03 (June 12, 2026)

The third drop ([official announcement](https://www.war.gov/News/Releases/Release/Article/4515408/department-of-war-publishes-third-release-of-unidentified-anomalous-phenomena-f/)). **72 files — 53 PDFs, 10 images, 6 videos, 3 audio — ~5.5 GB on disk.** Highlights: CIA Cold War-era assessments (including **Project Blue Book Special Report No. 14**), the Department of War "Western United States Event" narrative statements, FBI orb-sighting reports with digital renderings & recreation video, and NASA **Apollo-16** debriefing audio plus a 1962 Gordon Cooper interview excerpt.

#### ⚠️ Still no public bulk mirror

Same story as Release 02 — checked 2026-06-15:

- **[vfp2/pursue-ufo-files](https://github.com/vfp2/pursue-ufo-files)** remains Release-01-only (don't use its index for R02+).
- **[warufo.com](https://warufo.com/)** / **[ufo-declassified.com](https://ufo-declassified.com/)** track the new files but still front `war.gov/medialink/` links.

#### Acquisition notes

- The site's single-source-of-truth CSV was **renamed** from `uap-csv.csv` to **`uap-data.csv`** (`https://www.war.gov/Portals/1/Interactive/2026/UFO/uap-data.csv`) and gained a leading `Featured` column — column indices shifted by one if you had a Release-02-era parser.
- PDFs/images: headed-browser in-page fetch from `war.gov/medialink/ufo/061226/release_03/documents/…` (Akamai still 403s everything cloud/curl).
- Videos/audio: DVIDS `.m3u8` → CloudFront `.mp4`, as with Release 02. Note the CSV keys media by dvidshub **video-id** while the CloudFront assets are named by DVIDS **media-asset-id** (`DOD_…`) — resolve one to the other via the dvidshub video page or playlist.

### Release 04 (July 10, 2026)

The fourth drop. **40 files — 14 PDFs, 3 images, 19 videos, 4 audio — ~1.7 GB on disk** (28 Department of War, 7 NASA, 2 CIA, 2 DOE, 1 FBI; per DoW ~48% are partially redacted to protect witnesses and platforms). Highlights: the 1948 **Project Sign** progress report and 1948–49 USAF "Analysis of Flying Object Incidents" studies, the 1966–67 Air Force **Project Blue Book review committee** deliberations, CIA 1955 "flying saucer" debriefs, NASA **STS-80** unidentified-object imagery from Space Shuttle Columbia, 19 Navy / NORTHCOM / INDOPACOM UAP FLIR clips (Yellow Sea, East & South China Sea, both US coasts), **Apollo 14 & 17** crew-debriefing audio, and DOE Los Alamos (1949) & Pantex (2015) records.

#### ⚠️ Still no community mirror (checked 2026-07-16) — but war.gov now ships bulk zips

Community mirrors have not kept up — the Release-01 repos are unchanged and the analysis sites ([warufo.com](https://www.warufo.com/) now covers all four releases) still link back to war.gov.

The good news: for Release 04, **war.gov itself offers bulk zip bundles** (e.g. `release_04_documents_071026.zip` ~238 MB and `uap_release04_videos_071026.zip` ~1.5 GB), so a browser on a residential IP can grab the whole drop in two downloads — no more per-file scraping. The Akamai rules are unchanged, so curl/cloud egress still 403s; and the video zip names files by DVIDS **media-asset-id** (`DOD_…`), so you still need the dvidshub video-id → asset-id mapping (via each video's `.m3u8`) to match them back to the CSV rows. The CSV now serves with a `?release=4` query parameter but keeps the same 29-column layout as Release 03.

### Release 05 (Aug. 7, 2026)

The fifth drop. **41 files — 22 PDFs, 3 images, 16 videos, no audio — ~646 MB on disk** (19 Department of War, 17 FBI, 2 CIA, 2 Department of State, 1 Executive Office of the President — the first EOP record in the series). Highlights: the CIA's 1965 "Unidentified Flying Object Reported near Puerto Rico" report and briefing notes for Walter Elder, two November 1963 **State Department cables from Brazil** plus the **National Aeronautics and Space Council inquiry** into the Bahia incident they describe, the Department of War's 1953 film analysis of unidentified objects, an intelligence review of the 1946 **"Ghost Rocket"** incidents and an Air Materiel Command flying-object report, FBI FD-302 witness reports on 2002 and 2023 **triangle** sightings with the Bureau's digital renderings of them, and 15 Department of War sensor clips plus one FBI video.

#### Acquisition: bulk zips again (mirrored 2026-08-27)

Same path as Release 04 — **war.gov ships the drop as two zips**: `release_05_Aug_07_documents.zip` (25 documents + images, already under their war.gov canonical names) and `uap_videos_080726.zip` (16 videos under raw CloudFront `DOD_111887xxx.mp4` names). Download both from a browser on a residential IP; Akamai still refuses curl and cloud egress.

- To rename the videos you no longer need the `.m3u8` detour: each CSV row carries a **DVIDS Video ID**, and the plain page `https://www.dvidshub.net/video/{id}` exposes the CloudFront asset path (`…/{DOD_nnn}/{DOD_nnn}.mp4`) in its HTML — curl-able, no login. All 16 resolved 1:1 against the zip contents, and every DVIDS page title matched its CSV title.
- The CSV (`uap-data.csv?release=5`) keeps the 29-column Release-03 layout.
- Community mirrors were not re-surveyed for this release; with war.gov shipping its own zips, the portal itself remains the practical bulk source.

---

## 🇧🇷 Brazil — Arquivo Nacional

Brazilian Air Force / Ministry of Defense files including the Operação Prato / Colares (1977–78) materials. The Arquivo Nacional catalogues 893 documents (1952–2023), with declassifications rolling since 2009; the IA item below holds a subset — **236 files / 4,785 pages** once its RAR archives are unpacked. (An earlier version of this guide repeated a "~20,000+ pages" figure that nothing in the mirror supports.)

**[IA: `BrazilianUFOFiles`](https://archive.org/details/BrazilianUFOFiles)** — multi-item collection (142 zipped sub-items + their metadata + OCR derivatives). Originally pulled 1.5 GB / 846 files with default flags; re-ran with `--source=original` to get the 232 originals — 231 PDFs plus IA's own `__ia_thumb.jpg` — (~1.9 GB) and skip OCR/thumbnail/metadata noise.

**Don't stop at the PDFs.** The item also ships **five RAR archives of page scans** that are easy to overlook next to the loose PDFs — they hold 224 pages, including *both parts* of the Operação Prato mission reports (151 + 54 pages), Diretriz Específica 04/89 (the UFO-reporting procedure for airspace-control units), the confidential NPA-09 procedure with its witness questionnaire, and archived press clippings. Reassemble each with `img2pdf`, which embeds the JPEG scans verbatim so every page stays byte-identical to the source. And drop `__ia_thumb.jpg` from any index you build: IA generates one per item and `ia download` pulls it alongside the originals, so it turns up as a fake "image" in every IA-sourced folder.

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

This guide was assembled by attempting each of the above downloads in sequence on 2026-05-15, with the PURSUE Release 02 section added 2026-05-25, Release 03 added 2026-06-15/07-16, Release 04 added 2026-07-16, and Release 05 added 2026-08-29 (mirrored 2026-08-27). Status reflects what worked *those days* — LFS budgets get topped up, mirrors get taken down, Akamai rules change. If you find a mirror that's flipped status, PRs welcome.
