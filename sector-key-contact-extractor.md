---
name: sector-key-contact-extractor
description: extract sector names and ordered key contacts from supplied gilbert + tobin sector urls, verify the extracted names against the canonical published profiles csv when available, and present one full inline table covering all supplied sectors before creating an excel workbook only when the user explicitly asks for it. use when the user provides a list of sector names or sector urls from gtlaw.com.au and wants a fast url-first workflow for reviewing key contacts across multiple sectors, then exporting the reviewed results into a spreadsheet with separate tabs for each sector.
---

# Extract sector key contacts from supplied sector URLs

Use this skill for a fast URL-first sector workflow.

The normal workflow is:

1. The user provides a list of sector names or sector URLs.
2. Resolve each sector to its live Gilbert + Tobin sector page on `gtlaw.com.au`.
3. Read the sector heading from the live page.
4. Extract the ordered people list from the live public page.
5. Build one full inline table covering all supplied sectors.
6. Wait for the user to visually review that inline table.
7. Only after the user explicitly asks, create an Excel workbook with a separate tab for each sector.

## Required inputs

Use these inputs:

1. A user-supplied list of sectors, sector names, and or sector URLs.
2. The canonical verification CSV named `Published profiles-Fri13March2026.csv` when name verification is needed and available inside the skill bundle.

Treat the supplied sector URL as the primary extraction source.

Treat the CSV as the primary source for canonical name verification when verification is performed.

## Accepted inputs

Support these inputs:

1. One or more live sector URLs from `https://www.gtlaw.com.au`.
2. A list of Gilbert + Tobin sector names that can be resolved to sector URLs.
3. A mixed list of sector names and sector URLs.

Prefer live sector URLs because they are faster and often more accurate for this workflow than visually reading CMS screenshots.

## URL-first extraction rules

For each supplied sector:

1. Use the live public sector page as the extraction source.
2. Read the sector name from the page heading.
3. Identify the ordered people list shown on the page.
4. Extract each person name in exact top-to-bottom order.
5. Preserve ordering exactly as displayed on the live page.
6. Do not infer extra names from unrelated page sections.
7. If the page appears to contain multiple people lists, use the main sector people list that best represents the key contacts shown for that sector.
8. If the intended list is unclear, mark the affected rows as needing manual review.

## Multi-sector workflow

When the user supplies multiple sectors, process them into one combined inline table.

1. Treat each supplied URL or sector name as one sector input unless the user states otherwise.
2. Process all supplied sectors before presenting the review output.
3. Combine all extracted rows into one full inline table.
4. Group rows by sector and preserve the order of sectors given by the user.
5. Within each sector, preserve the exact top-to-bottom order from the page.
6. Do not create the Excel workbook yet.
7. Wait for the user to visually scan the combined inline table first.

## Verification rules

Verify each extracted name against `Published profiles-Fri13March2026.csv` when the CSV is available and verification is needed.

Build the canonical comparison name from:
- `First name`
- `Last name`

Match using exact full-name comparison first.

If an exact match is not found:

1. Check whether the extracted spelling may reflect a page-reading issue or formatting issue.
2. Prefer the closest plausible published-profile name only when the live-page evidence and CSV evidence together make the correction clear.
3. Otherwise mark the name as `Unverified`.
4. Do not silently normalise or auto-correct uncertain names.

If the task is primarily a fast extraction pass and verification has not yet been performed, state that clearly in the table.

## Inline review output

After all supplied sectors are processed, provide one combined inline table.

Use these columns for the inline review table:

- Sector
- Key Contact Order
- Extracted Key Contact Name
- Verified Key Contact Name
- Verified in Published Profiles CSV
- Verification Notes
- Source URL

Populate `Verified Key Contact Name` with the canonical CSV spelling where confirmed.

Use concise verification notes such as:
- Exact match in CSV
- Corrected from source ambiguity
- Unverified - no exact CSV match
- Not checked from CSV in this step
- Needs manual review

Do not include a `Component ID` column in this workflow.

## Final Excel output rule

Do not create the Excel workbook unless the user explicitly instructs that final step after reviewing the inline table.

When the user explicitly requests the final spreadsheet:

1. Create one Excel workbook containing all processed sectors.
2. Create a separate worksheet tab for each sector.
3. In each worksheet, include only these final columns:
   - Sector
   - Key Contact Order
   - Key Contact Name
4. Use the verified canonical name where available.
5. If a name remains unresolved, keep the best available name and flag the issue in the chat response.
6. Ensure every processed sector is included in its own worksheet tab.
7. Name each worksheet clearly using the sector name, shortened only if required by Excel sheet-name limits.

## Consolidation behaviour

Maintain a running consolidated dataset across the conversation for all supplied sectors processed for this task.

If the user adds more sector URLs later in the same task, append them to the existing consolidated dataset unless the user asks to start again.

Use the live source URL for traceability in the inline table.

## Quality controls

Before presenting results:

1. Check for duplicated names caused by extraction error.
2. Check that ordering is sequential and complete within each sector.
3. Check that the sector name matches the live page heading.
4. Re-check ambiguous names before marking them verified.
5. Be explicit about uncertainty.
6. Keep the inline table easy to scan across all supplied sectors.
7. Prefer the supplied live URL workflow over screenshot-based extraction for this skill.

## Response style

For the multi-sector review step:
- Provide a short confirmation
- Then provide the full combined inline table
- Then note any unresolved or unclear names in plain text

For the final Excel step:
- Provide the download link to the spreadsheet
- Confirm that the workbook contains separate tabs for each sector
- Summarise any unresolved names in plain text
