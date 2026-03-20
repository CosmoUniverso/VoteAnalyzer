# VoteAnalyzer

Mobile-first grade tracker for Italian students. Single HTML file, no dependencies to install, no server required.

## Features

- Add grades with subject, value, semester, date, weight and school year
- Weighted average calculation
- Grade distribution chart with half-grade steps (6, 6.5, 7, 7.5, ...)
- Per-subject average breakdown
- Multi-year tracking with trend charts
- Q1 vs Q2 comparison within the same year and across years
- Peer comparison — import a classmate's CSV and compare averages, subject breakdown and grade distribution side by side
- Export and import via JSON and CSV
- All data stored locally in the browser (localStorage), nothing leaves the device
- Works offline after first load
- Installable as a PWA on Android and iOS

## Usage

Download `index.html` and open it in any modern browser. No build step, no installation.

To install as a mobile app:

- **Android** — open in Chrome, tap the menu, select "Add to Home Screen"
- **iOS** — open in Safari, tap the share button, select "Add to Home Screen"

## File Structure

```
VoteAnalyzer/
├── index.html          # The entire application
├── README.md
├── docs/
│   ├── CSV_FORMAT_IT.md    # CSV format specification (Italian)
│   └── CSV_FORMAT_EN.md    # CSV format specification (English)
└── examples/
    ├── voti_2024-25.csv    # Example grade file
    └── voti_2025-26.csv    # Example grade file
```

## CSV Format

Grades can be imported and exported as CSV. The expected format is documented in full in [`docs/CSV_FORMAT_IT.md`](docs/CSV_FORMAT_IT.md) (Italian) and [`docs/CSV_FORMAT_EN.md`](docs/CSV_FORMAT_EN.md) (English).

Quick reference:

```csv
"Materia","Voto","Quadrimestre","Data","Note","Peso","Anno"
"Matematica","8.5","1","2025-11-10","Verifica scritta","100","2025/26"
```

| Column | Required | Notes |
|---|---|---|
| `Materia` | yes | Subject name |
| `Voto` | yes | `1`–`10`, step `0.5`, period as decimal separator |
| `Quadrimestre` | yes | `1` or `2` |
| `Data` | yes | `YYYY-MM-DD` |
| `Note` | no | Free text |
| `Peso` | no | Integer `1`–`100`, defaults to `100` |
| `Anno` | no | Format `2025/26`, inferred from date if missing |

## Peer Comparison

The Compagni tab allows side-by-side comparison with classmates without sharing data through any external service.

1. Go to **Andamento → Compagni**
2. Enter the classmate's name and import their CSV
3. They do the same with your CSV, exported via the "Esporta il mio CSV da condividere" button

Peer data is stored separately from your own grades and persists independently.

## ClasseViva

ClasseViva (Spaggiari) does not expose a public API for students. Grades must be entered manually or imported via CSV. See [`docs/CSV_FORMAT_IT.md`](docs/CSV_FORMAT_IT.md) for instructions on building a compatible CSV from ClasseViva data.

For grades older than the previous school year, ClasseViva does not provide access through the student interface. Contact the school secretary's office to retrieve historical records.

## Browser Compatibility

Tested on Chrome, Firefox, Safari and Edge. Requires a browser with support for `localStorage`, `Blob`, `FileReader` and `Canvas`. All major mobile browsers on Android and iOS are supported.

## License

MIT
