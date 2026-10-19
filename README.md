# BQG — Bill of Quantity Generator (PWA prototype)

Responsive, installable, offline-capable frontend prototype for construction cost estimation. Designed for phone, tablet and desktop, using the BQG navy/teal identity and Manrope/DM Sans typography.

## Run locally

Serve this folder over localhost or HTTPS (service workers do not work from `file://`). For example:

```bash
python3 -m http.server 8080
```

Open `http://localhost:8080`. On GitHub Pages, publish the folder contents from the repository root or configure Pages to the folder. HTTPS is required for PWA install/offline service-worker functionality.

## First sign-in

- User ID: `admin`
- Default password: `minda@123`
- The first login requires setting a new password.

## Included in this test build

- Local prototype authentication with password hashing using Web Crypto; first-login password change.
- Local user creation, roles (Admin/QS Lead, Estimator, Viewer), company profile/address/tax/contact/logo settings.
- Project setup for Hulhumalé private, HDC, and other government land; HDC premium and commercial allocation; mixed-use, row houses, social/economic/affordable housing, private residence, schools and other building types.
- Foundation type, shallow groundwater/dewatering, EIA, geotechnical report, permit, authority submissions, strata and as-built allowances.
- Manual cost item entry and CSV import/export (CSV opens in Excel). The import template is downloadable.
- Amenity rows with custom labels, quantity, location/type and unit cost, including multiple pools and private/penthouse amenities.
- Project-duration based accommodation, utility, staff and site preliminaries; mobilization and temporary site costs.
- Blind BOQ using configurable benchmark rates, plus detailed BOQ from quantities × (material + labour + equipment/other); markup, contingency, FX buffer, tax and USD conversion.
- Client-facing print/PDF view and Excel-compatible CSV estimate export; internal calculation transparency in app only.
- Service worker cache for offline app shell after the first successful load.

## Important limitations — do not treat as production-ready

This is a frontend-only prototype. It stores data in the browser on the current device. It does not yet sync data across users/devices, provide a server/database, encrypted server-side credentials, audit logs, backups, robust role enforcement, real XLSX binary parsing/export, drawing takeoff, or validated Maldives-specific rates/statutory calculations. CSV import/export is supported; export to Excel-compatible CSV is intentionally used so it works offline without a third-party library. PDF is produced via the browser's Print > Save as PDF.

The initial admin password is a development bootstrap credential. Change it immediately. Client-side hashing is not equivalent to secure server-side authentication and cannot protect against a device/browser administrator. Before real use, add a backend (e.g. Node API + PostgreSQL/SQLite), server-side password hashing (Argon2id/bcrypt), session security, RBAC on the server, TLS, backups, audit trail, and a controlled sync strategy. Benchmark rates, tax and measurement rules need QS validation. Example blind benchmark rates are placeholders only.

## Rate CSV format

Header: `code,name,category,unit,materialRate,labourRate,equipmentRate,quantity`

Numbers are MVR rates per stated unit. Quantities are default blank/zero unless supplied. Import replaces the current rate library after confirmation.


## Advanced Blind BOQ: geometry and preliminary GFA

The Project Setup page includes optional geometry inputs for a preliminary floor-plate and gross-floor-area estimate:

- External building perimeter and assumed rectangular aspect ratio (or custom ratio).
- Total above-ground height and editable minimum/maximum floor-to-floor heights. The app estimates floor count from the midpoint of the entered range when a total height is supplied.
- Plot coverage percentage, plot ratio / FSI, optional minimum target GFA and a user-entered GFA adjustment.
- Location options: Malé, Hulhumalé, Local island and Remote island.
- A toggle to use the calculated GFA as the project built-up area.

**Formula and limitations:** for a rectangular footprint with perimeter `P` and length-to-width ratio `r`, estimated footprint area is `P² × r / (4 × (r + 1)²)`. The app caps this footprint at the entered plot coverage limit if both plot area and coverage are provided. Estimated GFA is adjusted above-ground footprint × estimated above-ground floors, plus the entered basement footprint allowance. Potential allowable GFA is plot area × entered plot ratio / FSI. These are early-stage geometric estimates only; they do not establish the legal GFA, minimum development requirement, permitted height, setbacks, coverage or exclusions for a specific plot. Enter the values from the applicable authority/site approval and verify them with the design team.

Floor-height values are editable planning assumptions (initially 3.0–3.4 m for Malé/Hulhumalé and 2.8–3.3 m for local/remote islands), not statutory requirements.

## Blind BOQ GFA calculator and custom summary works (prototype update)
- The dashboard Blind BOQ card has an expandable geometry-based GFA calculator. Enter building perimeter, aspect ratio, floor height range or floor count, basement count, plot area, plot coverage and plot ratio where known.
- `Calculate GFA` displays a preliminary footprint, estimated floors, total GFA and potential allowable GFA. `Use GFA for blind estimate` copies the resulting area into the quick estimate and project area fields.
- The Project setup page retains its advanced GFA inputs for the project workspace. Authority controls are not hardcoded as universal statutory values; verify the current plot-specific HDC, Malé City Council or island-council rules.
- Amenities & scope includes custom work items. Each item can be assigned to an existing BOQ summary heading, and administrators can add new headings. Custom works are included in the estimate totals and client-facing report/CSV export.
- This is still a browser-only prototype. Rates, benchmark data, authority rules and measurement assumptions need QS validation; no official regulatory approval or measured BOQ is implied.
