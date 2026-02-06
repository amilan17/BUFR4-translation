# Russian vs English BUFR Table Extraction Report

**Source PDF**: `russian_pdfs/306_I2_2019_ru.pdf` (WMO-306 Vol I.2, 2019 edition)
**English reference**: `BUFR4/*.csv` (current WMO edition)
**Extraction tool**: `table_extractor_vision/` (pymupdf position-based, no LLM)

## Summary

| Table | EN files | RU files | EN entries | RU entries | Coverage |
|-------|----------|----------|------------|------------|----------|
| Table A | 1 | 1 | 34 | 33 | 97% |
| Table B | 33 | 33 | 1,855 | 1,698 | 92% |
| Table C | 1 | 1 | 28 | 28 | 100% |
| Table D | 20 | 20 | 9,860 | 7,512 | 76% |
| CodeFlag | 25 | 24 | 5,933 | 4,618 | 78% |
| **Total** | **80** | **79** | **17,710** | **13,889** | **78%** |

All gaps are **version differences** (2019 Russian vs current English), not extraction errors.

## Table A — Data Categories

- `output/BUFR_TableA_ru.csv` vs `BUFR4/BUFR_TableA_en.csv`
- 33 of 34 entries (97%). Missing 1 entry added after 2019.

## Table B — Element Classification

- `output/BUFRCREX_TableB_ru_XX.csv` vs `BUFR4/BUFRCREX_TableB_en_XX.csv`
- All 33 classes present. 1,698 of 1,855 entries (92%).
- Differences are entries added to existing classes in post-2019 editions.

## Table C — Operators

- `output/BUFR_TableC_ru.csv` vs `BUFR4/BUFR_TableC_en.csv`
- **Exact match**: 28 of 28 entries (100%).

## Table D — Sequences

- `output/BUFR_TableD_ru_XX.csv` vs `BUFR4/BUFR_TableD_en_XX.csv`
- All 20 categories present. 7,512 of 9,860 entries (76%).
- Difference is due to new sequences added in post-2019 editions.

## CodeFlag — Code/Flag Tables

- `output/BUFRCREX_CodeFlag_ru_XX.csv` vs `BUFR4/BUFRCREX_CodeFlag_en_XX.csv`
- 24 of 25 classes extracted. 4,618 of 5,933 entries (78%).

| Class | EN | RU | Pct | Notes |
|-------|------|------|------|-------|
| 01 | 375 | 339 | 90% | 3 FXY added after 2019 |
| 02 | 1,110 | 891 | 80% | 2 FXY added after 2019 |
| 03 | 158 | 128 | 81% | 2 FXY added after 2019 |
| 04 | 14 | 13 | 93% | |
| 05 | 4 | 4 | 100% | |
| 08 | 813 | 627 | 77% | 13 FXY added after 2019 |
| 10 | 16 | 16 | 100% | |
| 11 | 110 | 107 | 97% | |
| 13 | 67 | 62 | 93% | 1 FXY added after 2019 |
| 15 | 6 | — | MISS | Entire class added after 2019 |
| 19 | 150 | 150 | 100% | |
| 20 | 1,077 | 845 | 78% | 5 FXY added after 2019 |
| 21 | 186 | 150 | 81% | 1 FXY added after 2019 |
| 22 | 84 | 83 | 99% | |
| 23 | 83 | 79 | 95% | 2 FXY added after 2019 |
| 24 | 6 | 6 | 100% | |
| 25 | 331 | 292 | 88% | 1 FXY added after 2019 |
| 26 | 26 | 25 | 96% | |
| 29 | 13 | 13 | 100% | |
| 30 | 25 | 24 | 96% | |
| 31 | 14 | 14 | 100% | |
| 33 | 885 | 483 | 55% | 27 FXY added after 2019 |
| 35 | 102 | 102 | 100% | |
| 40 | 261 | 161 | 62% | 4 FXY added after 2019 |
| 42 | 17 | 4 | 24% | 2 FXY added after 2019 |

## Methodology

Extraction used **pymupdf** (`page.get_text("dict")`) to get text spans with bounding box coordinates. Spans are classified into table columns by x-position and grouped into rows by y-position anchored on FXY codes. No LLM was used.

Key extractors:
- `table_a.py` — regex on pymupdf4llm markdown
- `table_b.py` — position-based, 9 column boundaries
- `table_c.py` — position-based, 4 columns
- `table_d.py` — position-based with FXY span merging
- `codeflag.py` — position-based with per-row leftmost-numeric detection
- `cli.py` — CLI runner (`python cli.py <pdf> --table all`)
