# CSV Format Specification — VotiApp

## Header

The first row must contain exactly the following column names, in this order:

```
"Subject","Grade","Semester","Date","Notes","Weight","Year"
```

Any file missing this header or using different column names will be rejected on import.

---

## Column Reference

| Column | Type | Required | Description | Accepted values |
|---|---|---|---|---|
| `Subject` | string | yes | Subject name | Any non-empty string |
| `Grade` | float | yes | Grade value | `1` to `10`, step `0.5` |
| `Semester` | integer | yes | Academic semester | `1` or `2` |
| `Date` | date | yes | Date of the grade | `YYYY-MM-DD` |
| `Notes` | string | no | Optional description | Any string, can be empty |
| `Weight` | integer | no | Grade weight in percent | `1` to `100`, default `100` |
| `Year` | string | no | School year | Format `YYYY/YY`, e.g. `2025/26` |

---

## Field Specifications

### Grade
- Decimal separator must be a **period** (`.`), not a comma.
- Valid values: `1`, `1.5`, `2`, `2.5`, ..., `9.5`, `10`
- Values outside the `1`–`10` range are discarded on import.

### Date
- Format: `YYYY-MM-DD` (ISO 8601).
- Example: `2025-03-15`
- Non-conforming formats (e.g. `15/03/2025`, `03-15-2025`) will cause the row to be skipped or misparsed.

### Semester
- Accepted values: `1` or `2` only.
- General rule: September–January maps to `1`, February–June maps to `2`.

### Weight
- Integer between `1` and `100`.
- Represents the relative weight of the grade in the weighted average calculation.
- If omitted or left blank, defaults to `100`.
- Common values: `100` (full weight), `50` (half weight), `20` (low weight).

### Year
- Recommended format: `2025/26` (start year / last two digits of end year).
- If omitted, the app infers the school year from the `Date` field:
  - Month >= 9 (September or later): `year/year+1`
  - Month < 9 (before September): `(year-1)/year`

---

## Example

```csv
"Subject","Grade","Semester","Date","Notes","Weight","Year"
"Mathematics","8","1","2025-10-15","Written test - Algebra","100","2025/26"
"Italian","7.5","1","2025-11-03","Oral exam","100","2025/26"
"English","6.5","1","2025-11-20","Written test","100","2025/26"
"Computer Science","10","1","2025-12-10","OOP project","50","2025/26"
"History","9","2","2026-02-18","Oral exam","100","2025/26"
"Mathematics","6","2","2026-03-05","Class test","100","2025/26"
```

---

## Import Behavior

- Rows with a missing or non-numeric `Grade` are silently skipped.
- Rows with a missing `Subject` are silently skipped.
- Duplicate detection is based on the `id` field in JSON imports only. CSV imports always append all valid rows.
- If `Year` is missing, it is inferred automatically — no row is discarded for a missing `Year`.
- If `Weight` is missing or non-numeric, it defaults to `100`.

### Import via Home tab
Appends grades to the current dataset. Does not filter or override by school year.

### Import via Trend tab
Requires a school year string to be entered before import. The value entered in the field overrides any `Year` value present in the file, allowing bulk re-labeling of a CSV that lacks the `Year` column.

---

## Common Errors

| Symptom | Likely cause | Fix |
|---|---|---|
| Rows not imported | Comma used as decimal separator in `Grade` | Replace `,` with `.` |
| Wrong dates | Non-ISO date format in `Date` | Use `YYYY-MM-DD` |
| Wrong school year assigned | Missing `Year` column | Add the column or use the Trend tab import |
| Empty import | Header row missing or malformed | Ensure the first row matches the spec exactly |
| Weight ignored | Non-integer value in `Weight` | Use integers only (`100`, `50`, `20`) |

---

## Minimal Template

```
"Subject","Grade","Semester","Date","Notes","Weight","Year"
"","","","","","100",""
```

Save as `.csv` with UTF-8 encoding and populate one row per grade.
