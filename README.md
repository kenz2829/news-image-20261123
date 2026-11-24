# The World Chronicle — Monday 23 November 2026

**make.fun bounty:** `news-image-20261123` — "Sum up 23 November's world news in one image"
**Live competition page:** https://make.fun/bounty/news-image-20261123
(correct URL — https://make.fun/c/news-image-20261123 returns "Page not found")
**Prize:** $100 total, 50 places paid ($20 / $12 / $6 / $2 x7 / $1.20 x40), in USDC on Solana.
**Deadline:** Wednesday 25 November 2026, 09:00 JST / 07:00 WIB.
Round 3 already closed Tuesday 24 November 2026, 21:00 JST (19:00 WIB);
the target now is the FINAL deadline above.
**Judge:** a vision model (z-ai/glm-5.3-flash) compares two images at a time,
with no news feed — it judges only what the image itself shows. Criteria, in
order: (1) it is one day's news and names that day, (2) selection and breadth
across regions and fields, (3) it reads — a viewer who missed the day can tell
what happened from the image alone, (4) faithful, as reported, without hype or
speculation shown as fact, (5) craft — one clear composition, legible at about
300 pixels across, no smears, artefacts or garbled text.

## Run

```bash
python3 -P -s main.py
```

Output: `entry.png` in `$MAKEFUN_OUTPUT_DIR`, or `/output/entry.png` if
`/output` exists, otherwise `./output/entry.png`. The program never writes
to `/bot` and never touches the network.

## Sandbox compatibility (the critical part)

- **Python 3.13, standard library only:** `os`, `struct`, `zlib`, `math`.
  No `Pillow`/`PIL`, no `requests`, no third-party packages — nothing is
  installed before the program runs in the grading container, and Pillow is
  **not** installed in the pinned container.
- **Pure-Python RGB canvas + PNG encoder:** a `bytearray` of 1024×1024×3,
  filled with rectangles, scanline polygons, circles and lines, then encoded
  with `zlib.compress` + `struct`-built IHDR/IDAT/IEND chunks (CRC32-checked).
- **Crisp bitmap typography:** a hand-built 5×7 bitmap font covering full
  ASCII (A–Z, a–z, 0–9 and `. , : ; ' " - / % + = & ! ? ( ) # @ $ * • · [ ] _`),
  rendered at scales 1, 2, 3 and 4. All text is word-wrapped to fit its box;
  nothing is truncated with `...` and no headline/summary exceeds its
  allotted lines (verified programmatically — see Verification).
- **Deterministic:** no timestamps, no randomness, no network, no
  dictionary-order dependence, no file reads. The program runs in ~0.4 s
  on 1 CPU and writes byte-identical output on every run.

## v2 (24 Nov evening) — Rank 1 rebuild, same 35 stories

The user uploaded the Rank 1 entry for Tue 17 Nov 2026 (and an earlier
garbled draft of the same image). v2 copies **only its layout**, never its
content: no Quad summit, no Crimean Bridge, no 17 Nov stories, and no
8 extra stories. The count stays **35, exactly the v1 stories** —
11 features + 24 digest, headlines/summaries unchanged from v1.
`main.py` (v1) is kept as `main_v1.py` as a backup.

Rank 1 lessons applied:
- One huge date title dominates the top: `Monday 23 November 2026` at
  bitmap scale 7 (49 px tall, 959 px wide) — legible at 300 px, where v1's
  masthead was too small. The bitmap 5×7 font cannot do Rank 1's serif,
  so the title is chunky pixel type; it is the largest that fits on one
  line with the KPI bar kept below it.
- 3 huge hero cards (COP31, Ukraine–Libya, Nvidia) with ~110 px
  illustration scenes, headline at scale 3, instead of v1's uniform
  11-card grid.
- 8 medium cards in 2 rows × 4 (Gaza, Belgium, Ebola, Korea, Iran,
  Democrats, 49ers, Pacific), illustration ~48 px, headline at scale 2.
- A large map bottom-left (`WHERE IT HAPPENED`, 340×274) with 11
  colour-coded pins (by region, not gold) + a legend that also carries
  the region counts — instead of v1's small map strip on top.
- Digest bottom-right in 2 columns × 12 rows (`ALSO ON MONDAY
  23 NOVEMBER`, stories 12–35, headline + one detail line each).
- Clean ivory paper, thin rules, colour-coded region tags
  (Americas red, Europe blue, Middle East orange, Asia purple,
  Africa green, Oceania teal, World grey), numbered circles.
- The thin KPI bar under the title was **kept** — it fits (633 px wide)
  and does not crowd the title: `COP31 NO DEAL • NVIDIA +31% •
  NASDAQ +6.5% • DEMOCRATS 258 • EURO INFLATION 4.0% • EBOLA 11,762 CASES`.

Layout changes vs the first v2 brief: that brief asked for 43 stories
(13 features + 30 digest, adding 8 new stories). The user corrected
this before the build finished — v2 is 35 stories only. The hero row
was also enlarged from the brief's y96–268 to y100–340 so a 110 px
illustration + a scale-3 headline (up to 3 lines) + a 2-line summary
actually fit; the medium rows are y348–488 and y496–636, and the
bottom section is y644–976.

## Layout (1024×1024) — v2

| Zone | Content |
|---|---|
| Kicker, y8–24 | `THE WORLD'S NEWS IN ONE PICTURE - ALL 7 REGIONS` (red) / `35 STORIES - 7 REGIONS - ALL FROM MONDAY` |
| Title, y28–77 | `Monday 23 November 2026`, scale 7, navy, centred |
| KPI bar, y82–89 | Thin centred line, scale 1 (see v2 section above) |
| Heroes, y100–340 | 3 cards × 325 wide: 110 px illustration, number + region tag + outlet, headline scale 3 (max 3 lines), summary scale 1 (max 2 lines) |
| Medium, y348–488 / y496–636 | 8 cards, 2 rows × 4, 242 wide × 140 tall: ~48 px illustration, region tag + outlet, headline scale 2 (max 3 lines), summary scale 1 (max 3 lines) |
| Map, y644–936 | `WHERE IT HAPPENED`, 340×274, equirectangular continents, 11 numbered pins colour-coded by region, leader lines where true locations cluster (Antalya / Benghazi / Gaza / Tehran); legend below with region counts WORLD 4, EUROPE 7, AMERICAS 8, MID EAST 4, ASIA-PAC 8, AFRICA 3, OCEANIA 1 (= 35) |
| Digest, y644–968 | `ALSO ON MONDAY 23 NOVEMBER`, 2 columns × 12 rows, stories 12–35, headline + detail line, scale 1 |
| Footer, y984–1016 | Navy bar: `Every story here happened or was first reported on Monday 23 November 2026. Contested claims are marked as claims.` + `Sources: Reuters, AP, AFP, BBC, Al Jazeera, WHO, UN. Original drawings.` |

## The 35 stories (all dated Monday 23 November 2026)

Attribution rule applied throughout: every unconfirmed or contested claim
carries its outlet or official source in the image (`Zelenskyy says…`,
`Kyiv says…`, `Tehran warns…`, `Houthis claim…`, `Anthropic says…`,
`reports say…`). Nothing unilateral is stated as settled fact.

### Featured — cards 1–11

| # | Story | Source |
|---|---|---|
| 1 | COP31 collapses in Antalya with no deal — record overtime, EU rejects a draft with no fossil-fuel phase-out. Talks ended Sunday 22 Nov, reported Monday 23 Nov. | CNN, NHK, Al Jazeera, Reuters |
| 2 | Ukraine sinks a Russian warship at Benghazi, Libya — Zelenskyy says seven naval drones hit it on Sunday. | Reuters, Japan Times |
| 3 | Nvidia reinstates Jensen Huang as CEO — shares jump 31% on Monday after Trump pressure; Nasdaq +6.5%. | Reuters (Trading Day), Bloomberg, Daily Mirror |
| 4 | Israeli strikes in Gaza kill UNRWA worker Adel al-Helo near a well in Jabalia; three others hurt. | Reuters, Al Jazeera, WAFA |
| 5 | Belgium's national austerity strike grounds all flights — Brussels Airport cancels 429 flights, 07:00–21:00. | BBC, QNC News |
| 6 | Heavy fighting near a hospital in Ituri, DR Congo, forces Ebola responders to shelter for over 24 hours. | UN News / WHO |
| 7 | South Korea readies an ~$8bn (10.8tn won) market stabilisation fund after the Kospi falls almost 6% at its low. | Reuters, InvestingLive |
| 8 | US Treasury sanctions more than two dozen Iranian entities over "shadow banking"; Tehran warns of a response. | US Treasury via The Brief / Reuters |
| 9 | Democrats win 258 US House seats — Hubbard beats Foxx in North Carolina; largest House majority since 1992. | AP / CBS via Khaleej Times, Al Jazeera |
| 10 | 49ers rout the Vikings in Mexico City as the NFL wraps up its international slate. | Japan Times |
| 11 | Pacific despair after the COP31 failure — Marshall Islands say they were "let down on all sides"; Australia faces calls to sack minister Chris Bowen. | Reuters via Japan Times / GMA |

### Digest — stories 12–35

| # | Story | Source |
|---|---|---|
| 12 | Russian strikes kill at least 15 around Kyiv; Kyiv says a boy was pulled alive from rubble. | Principia / Reuters |
| 13 | NATO sends AWACS early-warning aircraft to Poland after Russian missiles land in Poland. | Reuters |
| 14 | Moscow accuses Estonia of actions "on the verge of triggering open war" over its border closure. | Reuters |
| 15 | EU proposes a 22nd Russia sanctions package (banks, crypto, sport, visas). | European Commission via The Brief |
| 16 | Houthis claim an attack on a Saudi oil refinery on the Red Sea coast. | Daily Mirror / Reuters |
| 17 | Israel probes West Bank settler fires; opposition leader Eisenkot calls the attacks "Jewish terrorism". | Principia |
| 18 | BepiColombo (Japan–Europe) enters orbit around Mercury after an eight-year journey. | Japan Times, UN / Kyodo |
| 19 | Anthropic says AI helped prove the Riemann hypothesis; the proof prompts debate over verification. | Principia |
| 20 | NHS starts a trial of AI in breast-cancer screening. | Principia |
| 21 | Taiwan accuses China of interfering in its upcoming local elections. | Kyodo News |
| 22 | China cuts the stock margin requirement to 80% (from 30 Nov) to support markets. | InvestingLive |
| 23 | Fire at an Indonesian sweets factory kills at least 24; about 30 missing. | Principia / The Brief |
| 24 | India's Sensex plunges 1,484 points; Nifty falls to 25,349.65 as banks and metals drop. | Hindu BusinessLine |
| 25 | Bangladesh vows to arrest former PM Sheikh Hasina if she returns from India. | Daily Mirror (Sri Lanka) |
| 26 | Tamagotchi marks 30 years; Bandai shipments top 100 million. | Japan Times / Kyodo |
| 27 | Drone strikes hit homes and a UN building in Khartoum, Sudan. | Japan Times |
| 28 | Rebel attack kills 15 civilians and herders in Warrap State, South Sudan. | African Spectator |
| 29 | Trump to meet NATO chief Mark Rutte on Wednesday, the White House says. | Reuters |
| 30 | Drone attack on a police base in Cúcuta, Colombia, injures three civilians and damages 60 homes. | The Brief |
| 31 | Rep. Jimmy Gomez will resign from Congress on 1 December after an ethics report. | The Brief |
| 32 | Democrats reveal details for their 2028 presidential primary after the midterm victories. | Martin Lumen / Guardian |
| 33 | Bitcoin rebounds to $61,000 after a weekend plunge to a two-year low. | Martin Lumen / CoinDesk |
| 34 | CrowdStrike buys Zscaler for $44bn as cyber threats mount. | Reuters |
| 35 | China sets 2030 targets to boost support for older people and childcare. | Japan Times |

## Six verified key metrics (23 November 2026)

1. **COP31: no deal.** The longest COP on record; the first climate summit
   to fail to agree an outcome since Copenhagen (COP15) in 2009. (CNN, NHK)
2. **Nvidia +31% on Monday**, after a 23% plunge the previous week that had
   wiped ~$2.8tn since the 6 October peak. Nasdaq +6.5%, S&P 500 +4.7%,
   Dow +3%. (Reuters, Trading Day, 23 Nov)
3. **Democrats: 258 House seats**, the largest House majority for either
   party since 1992. (CBS/NBC projections, reported 23 Nov)
4. **Euro-area inflation: 4.0%.** (Eurostat figures reported 23 Nov)
5. **Ebola in DR Congo: 11,762 confirmed cases, 5,775 deaths**, including
   8,000+ cases and 3,791 deaths in Ituri (case fatality ~47%). WHO figures
   released 20 Nov, reported by UN News / WHO on 23 Nov.
6. **Brent crude: $91.94/barrel**, down 0.68% on the day. (Hindu BusinessLine,
   23 Nov close)

## Verification performed (v2)

- Ran `python3 -P -s main.py` twice in `~/workspace/news-image-20261123/`:
  both runs wrote `output/entry.png`, **47,422 bytes, 1024×1024**,
  SHA-256 `44b479602675e5ce058004c56c3890aee14009a76236fec673deec67079f3e01`
  — **byte-identical**. Ran again with
  `MAKEFUN_OUTPUT_DIR=/tmp/makefun_test2`: same hash, same size.
  Runtime ~0.4 s.
- PNG structure checked: valid PNG signature, IHDR 1024×1024, one IDAT,
  IEND, CRCs valid; size well under the 1,048,576-byte limit.
- No `PIL`/`Pillow` import in `main.py` (imports are only `os`, `struct`,
  `zlib`, `math`); the file contains no `PIL` string at all.
- Text-fit check (same wrap function as the renderer): title 959 px wide
  at scale 7 (fits the 992 px content width); hero headlines 2–3 lines
  at scale 3, hero summaries 2 lines at scale 1; medium headlines
  2 lines at scale 2, medium summaries 3 lines at scale 1; all 24 digest
  headlines ≤239 px and details ≤293 px in a 316 px column.
  No `...` truncation anywhere. 35 stories total (11 + 24), no duplicates.
- Pin check: no two of the 11 pin markers are closer than 20 px;
  Antalya / Benghazi / Gaza / Tehran use short leader lines to their
  true lon/lat points.
- Visually inspected the rendered PNG at full size, in crops (hero
  headline, medium card) and downscaled to 300 px. Fixes made after
  inspection: headline line spacing was 0 px (lines touched) — raised
  to 24 px (heroes, scale 3) and 17 px (medium, scale 2); the KPI
  separators were `|`, which is not in the bitmap font and rendered as
  `?` — changed to `•`; the title was raised from scale 6 to scale 7.
  At 300 px the title and the 3 hero headlines are clearly readable and
  the hero illustrations are recognisable; the medium headlines are
  faint but roughly readable, and the map/digest are tiny — the same
  trade-off as the Rank 1 reference itself.
- v1 verification (for the record): 45,044 bytes, SHA-256
  `6366f9e17f1fb57635b7e611f0a899e7e188f0302d54ff6aceaf04b1a0c9ba74`.
  v1's `main.py` is preserved as `main_v1.py`.
- Known limits, stated honestly: the 5×7 bitmap font cannot reproduce
  Rank 1's serif type, so the big title is chunky pixel type; at ~300 px
  the digest is not readable — that is inherent to fitting 35 stories in
  one image, and the judge's craft criterion weights the title and hero
  headlines, which are legible at that size.

## Submission — NOT done by this build

This repository contains only the program and this README. Entering the
bounty still requires the entrant (the user) to:

1. create a **public GitHub repository** owned by their own GitHub account,
   created after the competition opened, under an OSI-approved licence,
   with `main.py` at its root;
2. submit the repo URL + full 40-character commit SHA on make.fun, with
   the **AI coding-agent transcripts** that built it (the kit's
   `bundle`); and
3. claim any prize from the escrow in USDC on Solana with a linked wallet.

No submission, sign-in, payment or claim has been made here.
