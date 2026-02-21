# Changelog

All notable changes to the MAP Growth and Reading Fluency Dashboard are documented in this file.

## [1.5.1] - 2026-02-21

### Fixed
- **Critical bug**: Exported MG charts now render — added explicit 400px height to canvas containers (Chart.js requires parent height) and pinned CDN library versions (`chart.js@4.4.0`, `chartjs-chart-boxplot@4.3.3`, `chartjs-plugin-annotation@3.1.0`)
- **Critical bug**: MAP Growth Grade Level export now renders all 6 charts correctly (was broken due to `dataStore` reference in standalone HTML)
- **MAP Growth Homeroom export** now includes all 6 charts per course (RIT box plot, Growth box plot, Met Growth bar, Proj vs Obs, Efficiency, Scatter) — previously only had 2
- MAP Growth Homeroom export now includes branded header with school logo, matching the grade level export layout
- MAP Growth Homeroom export table is now scrollable with sticky header (max-height: 400px) — previously displayed as one long unsorted table
- Export chart scripts now wrapped in try-catch with a visible error message if Chart.js fails to load (e.g., no internet)
- **Critical bug**: Fixed `SyntaxError: '' string literal contains an unescaped line break` in exported MG reports — `\n` inside `getExportChartHelpers()` template literal was being interpreted as a literal newline instead of an escape sequence

### Changed
- All report headers (MAP Growth + Reading Fluency, on-screen and exports) now use the user's selected branding color as a solid background instead of a gradient
- On-screen MAP Growth reports (School Dashboard, Grade Level, Homeroom) now dynamically reflect the selected branding color via `var(--brand-primary)` — previously hardcoded green
- MAP Growth Grade Level export body background changed from branded gradient to neutral `#f5f5f5` for better readability
- Exported MG reports now fill full browser width (removed `max-width: 1400px` constraint) — matches on-screen report layout
- MG Homeroom export styling unified with Grade Level export: matching header, course card layout, table padding, chart grid gap (20px), footer bar, reset CSS, responsive breakpoint, and print media query
- Export boxplot charts no longer show individual data point scatter behind the box plots (`itemRadius` set to 0)
- **Performance Level Labels** unified — merged "Assessment Labels" and "Report Card Labels" into a single group of 4 customizable levels; all internal NWEA names (e.g., Below Expectation, Approaching) now resolve to the same custom label via alias mapping
- Developing/Approaching performance badges and chart annotation labels now use white text instead of brown — consistent with all other performance levels across on-screen reports, exports, and student reports

## [1.5.0] - 2026-02-17

### Added
- **Normative data versioning** — NWEA norms now support 2015, 2020, and 2025 datasets with a year selector in the MAP Growth import dialog
- Populated 2020 and 2025 norms data (means + SDs) for Reading K-12, Language Usage 2-11, Math K-12, Science 2-10
- Dynamic norms labels across all charts via `{NORMS_YEAR}` token replacement
- **PDF export** — Individual student reports export as vector PDFs via jsPDF (no print dialog)
- **Batch PDF-to-ZIP** — Export all student reports as a ZIP of individual PDFs with progress bar
- **MAP Growth visuals on student reports**:
  - RIT trend line with national norm band (norm +/- 0.5 SD shaded region) per course
  - Projected vs. Observed Growth grouped bar chart (green/red based on met status)
  - Percentile gauge with Below Avg / Average / Above Avg zones using `normalCDF()` computation
- Attributions popup in footer with NWEA data source credits and links to norms PDFs

### Fixed
- Independent term selection per test type — MAP Growth and Reading Fluency now remember their own selected term

### Changed
- Renamed "Top Growth" / "Bottom Growth" to "Highest Growth" / "Lowest Growth" on School Dashboard
- Norms year defaults to 2025
- `changeNormsYear()` now refreshes all active views when the norms year is changed

## [1.4.0] - 2026-02-15

### Added
- **MAP Growth School Dashboard** — summary cards (Students, Grades, Courses, % Met Growth, Avg Growth, Avg Efficiency) with 10 charts: RIT/Growth distributions by grade, Met Growth by grade/course, Projected vs Observed, Growth Efficiency, RIT trends, Quartile distributions, and Highest/Lowest Growth performer tables
- **MAP Growth Homeroom Report** — per-homeroom student data tables with RIT/Growth/Met Projection/Norm comparisons, 6 charts per course (RIT box plot, Growth box plot, Met Growth bar, Proj vs Obs, Efficiency, Scatter), ZIP export
- **MAP Growth Interventions** — concern-level checkboxes, sortable tables with SDs Below column, XLSX export, per-course risk scoring (SDs Below National Norm 80% weight + Growth Shortfall 20% weight)
- NWEA national norms SD constant (`NATIONAL_NORMS_SD`) with standard deviations per grade/term/subject
- Tooltip (?) descriptions for all MAP Growth charts via `addChartInfoButton()`
- Grade and course filters for Highest/Lowest Performers tables
- XLSX export for performer tables
- Sortable columns on performer tables

### Fixed
- Student Reports showing duplicate students (stringified StudentID keys)
- Homeroom not showing on student reports (now resolves via `dataStore.studentIndex`)
- MAP Growth deduplication for multi-teacher duplicates
- Independent term filtering per MG view (School Dashboard, Grade Level Report, Homeroom Report)

### Changed
- Default landing page changed from MAP Reading Fluency to MAP Growth
- Separated import modal into distinct MAP Growth and MAP Reading Fluency sections
- Student Reports promoted to top-level tab (alongside MAP Growth and MAP Reading Fluency)
- ClassList columns renamed from `ID.`, `Class`, `HRTs`, `HRs` to `StudentID`, `HomeroomTeacher`, `ClassName`

## [1.3.0] - 2026-02-08

### Added
- **Progress Monitoring tab** — dedicated top-level tab with WCPM/Accuracy trends, Phonemic Awareness/Phonics tracking, box plot distributions by grade, percentile by grade charts
- **Growth Tracking tab** — term-over-term growth analysis for WCPM, accuracy, and foundational skill domains
- **Interventions tab** — students flagged for intervention with concern-level multi-select checkboxes (Critical/High/Moderate/Watch)
- Expectation bands on WCPM and Accuracy distribution charts (Below/Approaching/Meets/Exceeds zones)
- Term/Grade/Homeroom filters on Progress Monitoring tab
- Grade Level Report ZIP export (JSZip)
- RF Homeroom Report ZIP export (JSZip)
- Template CSV download for MAP Growth (`downloadMGTemplate()`)
- Template Excel download for MAP Reading Fluency (`downloadRFTemplate()`)
- `buildStudentIndex()` with ClassList homeroom override priority

### Fixed
- Term sorting across entire app — replaced 16 instances of `.sort()` with `.sort(sortTerms)` for correct Fall/Winter/Spring ordering
- Observed Growth Distribution boxplot clipping (added `grace: '15%'` for negative values)
- RIT Distribution by Grade y-axis minimum set to 100
- Tooltip button colors made consistent (gray scheme)
- Student ID moved to left of Student Name in intervention tables

### Changed
- PM distribution charts redesigned as proper box plots using `@sgratzl/chartjs-chart-boxplot`
- PM percentile charts converted to grade-level box plots colored by quartile band
- MG interventions table styling unified with RF interventions

## [1.2.0] - 2026-02-04

### Added
- **MAP Growth data support** — CSV import for NWEA MAP Growth assessment data
- MAP Growth School Dashboard with RIT box plots and growth distributions
- MAP Growth Grade Level Report with per-grade analysis
- Homeroom linking via ClassList sheet with override priority
- `MAP_REFERENCE_DATA` constant with grade-level expectations for all RF domains
- Helper functions: `normalizeGradeForLookup()`, `getFSExpectation()`, `getWCPMExpectationLevel()`, `getLanguageCompExpectationLevel()`

### Fixed
- Grade K sorting (now sorts first, not last) with `sortGrades()` helper
- Progress Monitoring data not showing (added correct column name fallbacks)
- Percentile by Term charts for Phonological Awareness and Phonics (added norms-based fallback)

### Changed
- Progress Monitoring charts organized by term (Fall/Winter/Spring) instead of date
- Foundational Skills section moved above Oral Reading section
- Dashboard restructured into color-coded sub-sections

## [1.1.0] - 2026-02-02

### Added
- **Comprehensive Dashboard Restructure** with categorized visualizations:
  - Oral Reading Benchmark section (WCPM, Accuracy, Lexile charts)
  - Foundational Skills Benchmark section (Domain Scores, Percentiles, ZPD distributions)
  - Progress Monitoring section with trend charts
  - Box-plot style distribution charts
- TestCategory filter (Benchmark / Progress Monitoring)
- ZPD distribution charts for Phonological Awareness and Phonics
- Dynamic sample size (n=) in all chart tooltips
- Auto-preview feature for single student selection
- Phonological Awareness and Phonics trend charts on student reports
- Branded student report header styling
- Chart-to-image conversion for PDF/HTML export
- Sortable At-Risk Students table with Grade K sorting first

### Fixed
- Student Report accuracy showing 1% instead of correct percentage (decimal conversion)
- Performance distribution charts checking wrong column
- At-Risk Students detection using wrong column fallback

### Changed
- Dashboard updated to consistent pastel color scheme
- Import button moved to header
- Added save confirmation toast

## [1.0.0] - 2026-01-28

### Added
- Initial release of the MAP Growth and Reading Fluency Dashboard
- **MAP Reading Fluency data import** from NWEA Excel exports (multi-term: Fall, Winter, Spring)
- **Benchmark Dashboard** with Foundational Skills and Oral Reading visualizations
- **Homeroom Reports** — printable per-homeroom foundational skills report cards
- **Student Reports** — individual student view with WCPM, accuracy, and skill progress charts
- **HTML export** — standalone HTML reports for homerooms
- IndexedDB data persistence with save confirmation
- Term overwrite confirmation dialog
- Drag-and-drop file upload interface
