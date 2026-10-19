## Problem Statement

At NLTH Pharmacy, BCS (Bar-Code Stock Replenishment) reads Lot and Expiry from a pack's GS1 DataMatrix. Many packs have no such symbol: in-house prepacks (ABX, Extemp, Prepack) carry text-only labels, and some manufacturer packs carry only an EAN-13 with no Lot or Expiry. Staff cannot give these packs a scannable GS1 code today. A self-made GTIN would also need a GTIN → Item Code table kept in sync across BCS, RxStation and every PC, which nobody wants to maintain.

## Solution

A small portable Windows app, run from the pharmacy shared drive, that prints a GS1 DataMatrix label carrying `(01)` an **Internal GTIN**, `(17)` Expiry and `(10)` Lot. The Internal GTIN is calculated from the Item Code (and Pack Volume for liquids), so scanning apps get the Item Code back with arithmetic and need no table ([ADR-0002](https://github.com/neirud926-prog/Tiny-2Dcode-Printing-App-Gemini/blob/main/docs/adr/0002-internal-gtin-scheme-v1.md)). Staff enter or scan an Item Code, pick a Lot from today's Stock Onhand (or type it), enter the Expiry, preview the label, and print silently to the PC's label printer.

## User Stories

1. As pharmacy staff, I want to type an Item Code and see the drug name on screen, so that I know I'm labelling the right item.
2. As pharmacy staff, I want lower-case Item Codes upper-cased automatically, so that I don't have to worry about Caps Lock.
3. As pharmacy staff, I want to scan a shelf QR label into the Item Code field and have the Item Code taken from it, so that I don't mistype codes such as `AIR 01`.
4. As pharmacy staff, I want to scan an out-patient (6-character) or in-patient (13-character) dispensing-bag label into the Item Code field, so that I can label from whatever I have in hand.
5. As pharmacy staff, I want the app to ask for Pack Volume (30 / 60 / 120 / 240 / 480 mL / Other) only for liquid items, so that each bottle size gets its own Internal GTIN.
6. As pharmacy staff, I want non-liquid items never to ask for a volume, so that the form stays short.
7. As pharmacy staff, I want to pick a Lot from today's Stock Onhand for the item, so that Lot and Expiry fill in automatically and match the stock system exactly.
8. As pharmacy staff, I want the Lot choices listed earliest Expiry first with their quantities, so that I pick the right physical pack quickly.
9. As pharmacy staff, I want a picked Lot and Expiry locked unless I choose "Type lot manually", so that a typo can't break the match with the stock system.
10. As pharmacy staff, I want to type a Lot by hand for prepacks and for items not in Stock Onhand, so that every pack can be labelled.
11. As pharmacy staff, I want to enter Expiry as 8 digits (`ddmmyyyy`), so that entry is fast from the keypad.
12. As pharmacy staff, I want a hint telling me to enter day 01 for month-only packs, so that I follow ADR-0001 without remembering it.
13. As pharmacy staff, I want Expiry shown as `dd MMM yyyy` (e.g. `01 DEC 2027`) on screen and on the label, whatever language Windows uses, so that dates are unambiguous.
14. As pharmacy staff, I want printing blocked for an Expiry that has already passed, so that expired stock never gets a fresh label.
15. As pharmacy staff, I want a warning, which I can override, when Expiry is within 3 months, so that short-dated stock is noticed.
16. As pharmacy staff, I want Lots limited to `A–Z 0–9 - / ( )` (lower case upper-cased, at most 20 characters), so that every scanner reads them the same way.
17. As pharmacy staff, I want a clear message when a Lot contains a space, which GS1 cannot encode, so that I understand why it is refused.
18. As pharmacy staff, I want to choose 1–99 copies, so that I can label a whole batch at once.
19. As pharmacy staff, I want a preview of the label at its real proportions, so that I see what will print.
20. As pharmacy staff, I want the app to decode its own code and show what it decodes to, with printing blocked if that differs from what I entered, so that a wrong label can never be printed.
21. As pharmacy staff, I want the label to print silently on my PC's label printer, so that I don't fight a print dialog.
22. As pharmacy staff, I want a Lot longer than 15 characters moved to its own line, so that it is never cut off.
23. As pharmacy staff, when an Item Code is not in the Item List, I want to choose between importing a newer Item List and saving the request as a Pending Label, so that work is not lost.
24. As pharmacy staff, I want Pending Labels rechecked after every Item List import and printable with one click, so that I can finish them later.
25. As pharmacy staff, I want no "print anyway" for unknown Item Codes, so that BCS never meets a label it would reject.
26. As the first user of the app, I want Setup to require an Item List import before anything else, so that Item Codes are always validated.
27. As pharmacy staff, I want the Item List import to show a summary (items, liquids, file date) and to refuse a bad file while keeping the previous list, so that a wrong file can't break the app.
28. As pharmacy staff on a new PC, I want Setup to ask only for this PC's printer and label size and to do a test print, so that each PC is ready in a minute.
29. As pharmacy staff, I want later launches to open straight to the label screen with a header showing the Item List date, Stock Onhand status and printer, so that I can start at once.
30. As pharmacy staff, I want a prompt at launch to import today's Stock Onhand, with a Skip button, so that lot picking is available when I want it.
31. As pharmacy staff, I want a Stock Onhand dated on another day to be unusable for lot picking, so that I never pick from stale stock data.
32. As a supervisor, I want every print recorded in a monthly CSV per PC (date/time, Windows user, PC, Item Code, Pack Volume, Internal GTIN, Expiry, Lot, copies), so that I can trace which Lots were labelled.
33. As a supervisor, I want every import recorded (who, when, which file, row count), so that I know where the data came from.
34. As a supervisor, I want the app's data files to be unreadable and uneditable by just opening them, and edited files to be detected and refused, so that nobody changes data by accident.
35. As pharmacy staff, I want several PCs to use the app from the shared drive at the same time without corrupting data, so that we don't have to take turns.
36. As pharmacy staff, I want the Item List, Stock Onhand and Pending Labels shared by all PCs while each PC keeps its own printer settings, so that one import serves everyone.
37. As pharmacy staff, I want the app to work with no internet access, so that it runs on the hospital network.
38. As pharmacy staff, I want a clear message if the app cannot write to its own folder, so that I know to move it.
39. As pharmacy staff, I want a Settings screen for printer, label size, offsets and log folder, so that a smaller label roll only needs a settings change.
40. As BCS and RxStation, I want to recover the Item Code from an Internal GTIN with arithmetic alone, so that no GTIN table is needed (BCS-Cross-Platform#16, Small-lable-printing#31).
41. As a developer of BCS or RxStation, I want a published test-case file of Item Code → Internal GTIN pairs, so that every copy of the encoder can be proven identical.
42. As the person deploying the app, I want a single portable `.exe` on GitHub Releases, so that I just copy it to the shared drive.

## Implementation Decisions

- **Platform:** Electron + TypeScript, React renderer, built with electron-vite and packaged by electron-builder as a Windows x64 **portable** `.exe` (no installer, no admin). Context isolation on; the renderer gets a narrow preload API and no Node access. No remote URLs; a strict content security policy.
- **Data folder:** `LabelData\` next to the `.exe`, resolved from the portable launcher's directory (`PORTABLE_EXECUTABLE_DIR`), falling back to a local folder in development. Contents: `items.dat`, `stock.dat`, `pending.dat`, `pcs\<PC name>.dat` (per-PC printer, label size, offsets, log folder) and `logs\`.
- **Label core (pure module, no Electron or file access):**
  - Internal GTIN codec exactly per ADR-0002: Pack Volume digit, prefix `04`, base-40 Item Code, mod-10 check digit, reserved ranges refused.
  - Expiry rules per ADR-0001: 8-digit `ddmmyyyy` entry, past dates refused, warning under 3 months, `dd MMM yyyy` display.
  - Lot rules: `A–Z 0–9 - / ( )`, upper-cased, max 20, space refused.
  - Scan-input parser: plain Item Code, shelf QR `ITEMCODE_BIN_…`, 13-character in-patient bag label (Item Code at characters 5–10).
  - Label builder: request + Item List + Stock Onhand + today → either validation errors or a label (Internal GTIN, GS1 element string, display lines).
  - Element-string decoder used as the self-check.
- **Element string:** FNC1, then `01`+GTIN, `17`+YYMMDD, `10`+Lot. Lot is last; no trailing separator.
- **Importers:** the Item List parser reads `item_phq.txt` (tab-separated, latin1, no header), keeping column 0 (Item Code), 1 (drug name) and 14 (base unit; `ML` = liquid), with per-row checks and a reject threshold. The Stock Onhand parser reads the Oracle report **"HA: INV Stock onhand with lot details"** (`.ps`, plain text): a `Key: value` header block (`Report Date: dd/mm/yyyy hh:mm:ss`, Organization Code, Subinventory Code), a blank line, then CSV with columns Cluster, Org Code, Subinventory, Item Code, Item Description, Lot Number, Expiry Date, UOM, Onhand Quantity, Unit Cost, Onhand Value. Item Code and Lot Number are wrapped as `="…"` and must be unwrapped; Expiry is `dd/mm/yyyy`; a description containing commas is recovered by reading the 4 leading and 6 trailing fixed columns.
- **Sealed data files:** `magic | HMAC-SHA256 | gzip(JSON)` with a key embedded in the app. This stops casual edits and detects any outside change. It is not encryption and is not tied to a PC or user.
- **Shared-drive safety:** every write goes to a temp file and is then renamed over the target, with retries for Windows file locks. Pending Label changes take a lock file, with a stale-lock timeout. Logs are one CSV per PC per month (`print-log-YYYY-MM-<PC>.csv`, `import-log-YYYY-MM-<PC>.csv`), append-only.
- **Rendering:** bwip-js `gs1datamatrix` as SVG. One module = 3 printer dots at 203 dpi (≈0.375 mm), so the symbol is about 7.5–8.3 mm. Layout: symbol left; Item Code bold with Pack Volume; `EXP`; `LOT`, which moves to its own full-width line past 15 characters. No drug name on the label. Default label 34.4 × 25 mm; size and offsets come from settings.
- **Printing:** a hidden window holding the label page, printed with Electron's silent print to the configured printer, with a custom page size in microns, no margins, and the copy count.
- **Startup:**
  - Missing or damaged `items.dat` → Setup step "Import Item List".
  - Missing PC settings → Setup step "Printer + test print".
  - No Stock Onhand for today → skippable prompt.
  - The app checks that it can write to its folder before anything else.

## Testing Decisions

- **Good tests** exercise behaviour through a seam's public interface (inputs → outputs), never internals.
- **Seam 1, label core:**
  - The shared test-case file of Item Code → Internal GTIN pairs. Codes are synthetic and cover every character and every observed pattern; this repo is public, so no real HA data is committed.
  - The ADR worked examples.
  - Expiry and Lot rules, Pack Volume, scan parsing, lot picking, and self-check mismatches.
  - An optional local test runs the codec over the real Item List when `ITEM_LIST_PATH` is set.
- **Seam 2, data folder (temporary directory, synthetic fixtures in the real formats):**
  - Imports and their row checks.
  - Sealed-file round-trip and tamper refusal.
  - The Stock Onhand report-date rule.
  - Per-PC settings, Pending Labels and the lock.
  - Atomic replace, and log appends.
- **Seam 3, render round-trip:** render with bwip-js, decode the image with zxing-wasm, and assert the decoded data equals the request.
- **UI:** a manual UAT checklist on the Pharmacy PC with the GX420t.
- **CI:** tests run on every push. A Windows job builds the portable `.exe`.

## Out of Scope

- The BCS and RxStation decoder changes (BCS-Cross-Platform#16, Small-lable-printing#31) and the RxStation prepack DataMatrix (Small-lable-printing#32).
- PIN protection, other symbologies, generating Lots or Expiries, and filling the form by scanning manufacturer barcodes.
- macOS/Linux builds and auto-update.

## Further Notes

- **Expected to be rare.** Internal GTINs are for the few items with no Manufacturer GTIN (unregistered items and clinical-trial drugs); everything with a Manufacturer GTIN keeps using it. In BCS an Internal GTIN counts as a GTIN like any other (e.g. for the manual-fill override).
- The default printer is the Zebra GX420t (203 dpi) on 34.4 × 25 mm labels. Physical print quality and scan tests on the UROVO DT50S are part of UAT.
- Decisions of record: [ADR-0001](https://github.com/neirud926-prog/Tiny-2Dcode-Printing-App-Gemini/blob/main/docs/adr/0001-month-only-expiry-day-01.md), [ADR-0002](https://github.com/neirud926-prog/Tiny-2Dcode-Printing-App-Gemini/blob/main/docs/adr/0002-internal-gtin-scheme-v1.md); vocabulary in [GLOSSARY.md](https://github.com/neirud926-prog/Tiny-2Dcode-Printing-App-Gemini/blob/main/GLOSSARY.md).

🤖 Generated with [Claude Code](https://claude.com/claude-code)


