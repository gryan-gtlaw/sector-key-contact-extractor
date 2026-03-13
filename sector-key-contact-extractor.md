---
name: sector-key-contact-extractor
description: extract sector names and ordered key contacts from uploaded gilbert + tobin sector cms png screenshots, verify extracted names against the canonical published profiles csv, and present a progressive inline verification table for each uploaded sector. use when the user uploads one or more sector screenshots from squiz matrix and wants key contacts captured in exact on-screen order, checked against the published profiles reference file, and eventually consolidated into a final excel spreadsheet only when the user explicitly instructs that final export step.
---

# Extract sector key contacts from CMS screenshots

Extract structured sector contact data from Gilbert + Tobin CMS screenshots.

## Required inputs

Use these inputs:

1. One uploaded PNG screenshot for a single sector CMS screen.
2. One canonical verification CSV named `Published profiles-Fri13March2026.csv`.

Treat the CSV as the primary source for name verification.

## Extraction rules

For each uploaded sector PNG:

1. Read the sector name from the CMS screenshot.
2. Identify the relevant Key Contacts repeatable component.
3. Capture the component ID exactly as shown, for example `rep-001`.
4. Extract each contact name in exact top-to-bottom order from the repeatable contact list.
5. Preserve ordering exactly as displayed in the CMS panel.
6. Do not infer missing names from rendered cards or other lower-page content if the repeatable panel is the intended source.
7. If the image is blurry or a name is visually ambiguous, mark the entry as uncertain before verification rather than guessing.

## Verification rules

Verify every extracted name against `Published profiles-Fri13March2026.csv`.

Build the canonical comparison name from:
- `First name`
- `Last name`

Match using exact full-name comparison first.

If an exact match is not found:

1. Check whether the screenshot spelling may be an OCR or visual-reading error.
2. Prefer the closest plausible published-profile name only when the image evidence and CSV evidence together make the correction clear.
3. Otherwise mark the name as `Unverified`.
4. Do not silently normalise or auto-correct uncertain names.

Use secondary evidence such as Outlook or Teams only if the user explicitly asks for a fallback check or if the CSV is missing or incomplete. Keep the CSV as the primary source.

## Interim output for each sector

After each sector PNG is processed, provide an inline verification table.

Use these columns for the interim table:

- Sector
- Component ID
- Key Contact Order
- Extracted Key Contact Name
- Verified Key Contact Name
- Verified in Published Profiles CSV
- Verification Notes
- Source Image Filename

Populate `Verified Key Contact Name` with the canonical CSV spelling where confirmed.

Use concise verification notes such as:
- Exact match in CSV
- Corrected from screenshot ambiguity
- Unverified - no exact CSV match
- Needs manual review

## Final Excel output rule

Do not create the final consolidated Excel spreadsheet unless the user explicitly instructs that final step.

When the user explicitly requests the final spreadsheet:

1. Consolidate all processed sectors into one Excel file.
2. Include only these final columns:
   - Sector
   - Key Contact Order
   - Key Contact Name
3. Use the verified canonical name where available.
4. If a name remains unresolved, keep the best available name and flag the issue in the chat response.
5. Ensure all sectors processed so far are included in the final workbook.

## Filename and consolidation behaviour

Maintain a running consolidated dataset across the conversation for all sector screenshots processed for this task.

Treat each uploaded PNG as one sector input unless the user states otherwise.

Use the screenshot filename in the interim table for traceability.

## Quality controls

Before presenting results:

1. Check for duplicated names caused by extraction error.
2. Check that ordering is sequential and complete.
3. Check that sector name and component ID are captured correctly.
4. Re-check visually ambiguous names against the CSV before marking them verified.
5. Be explicit about uncertainty.

## Response style

For interim sector processing:
- Provide a short confirmation
- Then provide the inline verification table

For the final Excel step:
- Provide the download link to the spreadsheet
- Summarise any unresolved names in plain text