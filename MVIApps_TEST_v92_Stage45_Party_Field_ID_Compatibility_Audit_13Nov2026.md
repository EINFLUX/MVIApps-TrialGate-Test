# MVIApps TEST v92 — Stage45 Party Field & ID Compatibility Audit

Date: 13 Nov 2026
Base: `MVIApps_TEST_v91_Stage45_Party_Endpoint_Path_Fix.html`

## Purpose
This is a narrowly scoped TEST frontend compatibility patch supporting the ongoing Production integration work. It does not deploy or modify Production services.

## Changes
1. Normalizes Cloud Party fields for the existing Stage45 UI:
   - `openingBalance` is mapped to `opening` only when `opening` is absent/empty.
   - `Dr` / `Debit` normalize to `Debit`; `Cr` / `Credit` normalize to `Credit`.
   - missing Party type defaults to `Customer`.
   - Party IDs are retained as strings in the cloud-read bridge.
2. Makes the active Stage45 Party list and Party detail Modify action preserve string IDs in inline handlers using URL-encoded string arguments rather than `Number(id)`.
3. Canonical conflict comparison ignores the duplicate `openingBalance` alias after mapping, preventing false conflicts solely from field-name/Dr-Debit representation differences.
4. Existing local Party records are not overwritten. Cloud-only records are appended to the TEST browser's local Party list as in v91. Local-only records are not uploaded.

## Preserved
- Stage45 Party List layout and renderer structure, except string-safe ID interpolation in active list handlers.
- Existing Create/Edit/Delete permission guard functions.
- Owner/Staff authentication adapter and TEST Business allowlist.
- Worker endpoint `/business/data/parties`.
- LocalStorage, IndexedDB and Google Drive Sync modules are not intentionally modified.
- No Cloud write routes are called by this Party read bridge.

## Validation performed
- 12 inline JavaScript blocks passed `node --check`.
- Synthetic normalization checks passed for string ID + `openingBalance` + `Dr`, and numeric ID + `opening` + `Debit`.
- Static source inspection confirms active Party list click/Modify/Delete handlers no longer coerce IDs through `Number(x.id)`.
- Static source inspection confirms Party detail modal Modify action preserves string ID.

## Not yet verified
- Browser validation of a Cloud-only Party with a non-numeric ID.
- Browser validation of Opening Balance display for Cloud-only `openingBalance` data.
- Owner/Staff Create/Edit/Delete permission behavior against the live TEST Worker.
- No Production deployment, D1 write, or real-data migration was performed.

## File integrity
- HTML: `MVIApps_TEST_v92_Stage45_Party_Field_ID_Compatibility.html`
- Size: 1,030,122 bytes
- SHA-256: `519fad58a3a97c712de39e33ab8f5dd00f822eb56ec2f7cd2e732c8007a2fc6e`
