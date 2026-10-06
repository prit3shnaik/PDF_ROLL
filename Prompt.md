Extract every voter record from this Election Commission Supplement PDF into a tab-separated .txt file.

SOURCE
- PDF: /workspace/2026-SUPPLEMENTGEN-S05-24-2-all_together-ENG-<N>-WI.pdf
- Output: /workspace/2026-SUPPLEMENTGEN-S05-24-2-all_together-ENG-<N>-WI.txt
- Same series as ENG-2 / ENG-3 (24-MORMUGAO). Do not reopen completed ENG files unless asked.

OUTPUT FORMAT
- TSV only. Header + data rows. No blank lines, no commentary, no section titles.
- Header exactly:
PART	SECTION NO	SR NO	NAME	RELATIVE NAME	RELATIVE RELATION	EPIC NO	AGE	HOUSE NO	GENDER
- Include Additions, then Deletions, then Modifications. Skip empty sections. Do not invent rows.

FIELD RULES
- PART = Part No. from page header (not Assembly Constituency number).
- SECTION NO = small box immediately right of Sr No. Not the #2 on modification cards.
- SR NO = serial in the left header box. Modifications show "#2  <sr>"; use only the serial.
- NAME / RELATIVE NAME = exact spelling, spacing, case, punctuation. Keep line-wrapped names as one space-joined string.
- RELATIVE RELATION = Father / Mother / Husband only (from Fathers/Mothers/Husbands Name).
- EPIC NO = full ID top-right (usually 10 chars). Recrop if clipped.
- AGE = integer. GENDER = MALE / FEMALE / THIRD GENDER (uppercase).
- HOUSE NO = exact printed string, including spaces, slashes, Roman I vs 1, quotes, commas. Never normalize (e.g. keep "2042/2" vs "204 2/2", "QTR. NO. M/135/I/4, TYPE 'B'", "MPT/ M/206/I/2").
- Ignore "Photo Available" and list headers.

METHOD (PDFs are image-only)
1. Render pages with pypdfium2 scale 3.0 (~4956x7014).
2. Crop each 3-column entry box. Do not OCR the full page (columns mix).
3. Typical box grid at scale 3.0 (verify if layout differs):
   cols: (168,1679), (1713,3236), (3273,4790)
   rows: (1143,1769), (1803,2429), (2463,3089), (3126,3749), (3786,4412), (4446,5072), (5106,5732), (5769,6392)
4. OCR each crop, then visually read every crop and correct OCR errors before writing the TSV.
5. Confirm row counts against page totals / last serial on Additions and Modifications.

VERIFY BEFORE FINISHING
- Every row has exactly 10 tab-separated columns.
- Unique SR NOs. PART matches header Part No.
- RELATIVE RELATION in {Father, Mother, Husband}; GENDER in {MALE, FEMALE, THIRD GENDER}.
- No Photo Available text in HOUSE NO. No trailing blank line.

OCR PITFALLS TO CHECK VISUALLY
- I vs 1 vs l; O vs 0 in EPIC (ROP0377465 not ROPOS77465).
- Names: SHUBHAM not SHJBHAM; HALLI not HALL; Mavlankar not Maviankar; RAJAPUT vs RAJPUT as printed.
- Gender printed as Female even if name looks male — keep printed gender.
- Modification Sr can skip numbers; extract only boxes that exist.
