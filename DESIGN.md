---
name: Aerospace & Earth Systems Instrumentation
colors:
  surface: '#f8faf5'
  surface-dim: '#d8dbd6'
  surface-bright: '#f8faf5'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f2f4ef'
  surface-container: '#ecefea'
  surface-container-high: '#e7e9e4'
  surface-container-highest: '#e1e3de'
  on-surface: '#191c1a'
  on-surface-variant: '#44483c'
  inverse-surface: '#2e312e'
  inverse-on-surface: '#eff1ec'
  outline: '#75796b'
  outline-variant: '#c5c8b9'
  surface-tint: '#4d662b'
  primary: '#162700'
  on-primary: '#ffffff'
  primary-container: '#283e06'
  on-primary-container: '#8faa67'
  inverse-primary: '#b3d089'
  secondary: '#4f6623'
  on-secondary: '#ffffff'
  secondary-container: '#cdea98'
  on-secondary-container: '#536a27'
  tertiary: '#1b2600'
  on-tertiary: '#ffffff'
  tertiary-container: '#2d3d00'
  on-tertiary-container: '#94a95f'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#cfeda3'
  primary-fixed-dim: '#b3d089'
  on-primary-fixed: '#111f00'
  on-primary-fixed-variant: '#364e15'
  secondary-fixed: '#d0ed9b'
  secondary-fixed-dim: '#b5d081'
  on-secondary-fixed: '#131f00'
  on-secondary-fixed-variant: '#384d0c'
  tertiary-fixed: '#d5ec9a'
  tertiary-fixed-dim: '#b9cf81'
  on-tertiary-fixed: '#151f00'
  on-tertiary-fixed-variant: '#3c4d0c'
  background: '#f8faf5'
  on-background: '#191c1a'
  surface-variant: '#e1e3de'
typography:
  headline-xl:
    fontFamily: Syne
    fontSize: 2.5rem
    fontWeight: '700'
    lineHeight: 3rem
    letterSpacing: -0.03em
  headline-lg:
    fontFamily: Syne
    fontSize: 2rem
    fontWeight: '600'
    lineHeight: 2.5rem
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Syne
    fontSize: 1.5rem
    fontWeight: '600'
    lineHeight: 2rem
    letterSpacing: -0.01em
  headline-sm:
    fontFamily: Syne
    fontSize: 1.125rem
    fontWeight: '600'
    lineHeight: 1.5rem
    letterSpacing: 0em
  body-lg:
    fontFamily: JetBrains Mono
    fontSize: 0.9375rem
    fontWeight: '400'
    lineHeight: 1.5rem
    letterSpacing: -0.01em
  body-md:
    fontFamily: JetBrains Mono
    fontSize: 0.8125rem
    fontWeight: '400'
    lineHeight: 1.375rem
    letterSpacing: -0.01em
  body-sm:
    fontFamily: JetBrains Mono
    fontSize: 0.75rem
    fontWeight: '400'
    lineHeight: 1.125rem
    letterSpacing: 0em
  label-md:
    fontFamily: JetBrains Mono
    fontSize: 0.75rem
    fontWeight: '600'
    lineHeight: 1rem
    letterSpacing: 0.06em
  label-sm:
    fontFamily: JetBrains Mono
    fontSize: 0.6875rem
    fontWeight: '500'
    lineHeight: 0.875rem
    letterSpacing: 0.08em
  telemetry-num:
    fontFamily: JetBrains Mono
    fontSize: 1.25rem
    fontWeight: '500'
    lineHeight: 1.5rem
    letterSpacing: -0.02em
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  gutter: 1rem
  gutter-desktop: 1.5rem
  margin: 1rem
  margin-desktop: 2rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 1rem
  space-lg: 1.5rem
  space-xl: 2.5rem
---

## Brand & Style

This design system establishes a high-precision, technical interface tailored for environmental monitoring, geospatial telemetry, and orbital sensor platforms. It marries the disciplined rigor of aerospace engineering with the nuanced palette of terrestrial ecological observation. 

### Brand Personality & Tone
- **Precision Instrumentation:** Uncompromising accuracy, high density of structured parameters, and absolute clarity.
- **Ecological Stewardship:** Calibrated vegetal and atmospheric tones that communicate Earth-monitoring without drifting into decorative or organic tropes.
- **Architectural Restraint:** Clean, unembellished surfaces that let telemetry, complex sensor tables, and orbital projections take precedence.

### Design Aesthetic
The visual language operates under a modern technical constructivist framework: crisp, feather-light divider lines, structural data grids, razor-sharp monospaced readouts via JetBrains Mono, and assertive, sculpted headings in Syne. The primary deep forest green (`#283E06`) acts as an operational anchor—used with strict restraint for focus indicators, critical status markers, and primary commit vectors—allowing high-albedo neutral surfaces to provide comfortable daylight legibility during prolonged analytical sessions.

## Colors

The color system maintains a disciplined 80/15/5 ratio: 80% light neutral grounds and crisp boundaries, 15% dark graphite typography and technical controls, and 5% targeted forest and sage accents. Green is treated as an intentional signal rather than a pervasive wash.

### Core System Tokens
- **Primary (`#283E06`):** Command vector dark green. Used for key interactive triggers, selected states, and core system identifiers.
- **Secondary (`#3B5110`):** Mid-tone operational olive. Applied to interactive hover states, secondary actions, and sub-system headers.
- **Tertiary (`#B7CD7F`):** Calibrated sage highlight. Reserved for telemetry badges, active toggle tracks, data visualization highlights, and low-contrast surface tints.
- **Neutral Ground (`#F8FAF5`):** Base canvas background, reducing visual fatigue compared to pure white while retaining crispness.
- **Surface Elevated (`#FFFFFF`):** Instrument cards, table wells, and module backgrounds.
- **Surface Sunk / Recessed (`#F1F5EB`):** Parameter input backgrounds, inactive track indicators, and data-density gutters.

### Functional Typography & Grid Lines
- **Text Primary (`#0F1710`):** High-density dark graphite ensuring maximum legibility across complex readouts.
- **Text Secondary (`#424B3E`):** Mid-graphite for telemetry labels, unit indications, and inactive column heads.
- **Border / Outline (`#DCE5D3`):** Precision 1px hairline delimiters that enforce clean spatial separation without heavy contrast weight.
- **Border Active / Focus (`#283E06`):** Direct feedback boundary for active data inputs and keyboard navigation targets.

## Typography

The typographic hierarchy establishes tension between human-engineered structural design (Syne) and rigid computational data output (JetBrains Mono).

### Architectural Rules
- **Structural Identity:** Headlines rendered in Syne are strictly geometric and tightly kerned. They articulate section ownership, module titles, and primary KPI groupings. All Syne usages remain sentence case or title case; never force full capitalization.
- **Telemetry Precision:** JetBrains Mono is the universal vehicle for values, tables, operational copy, labels, and metadata. Numbers are naturally tabular, preventing jitter during live real-time data streams.
- **Labels & Microcopy:** `label-md` and `label-sm` should be styled in uppercase with intentional tracking (`0.06em` to `0.08em`) to preserve legibility when paired alongside dense instrument graphics.

## Layout & Spacing

The layout is built for high-density desktop monitoring environments, employing a 12-column modular grid that maximizes operational viewport efficiency.

### Grid Construction & Margins
- **Desktop (>= 1280px):** 12-column fluid structure, constrained to a maximum width of `1800px` for ultra-wide command monitors. Standard horizontal margins are locked to `margin-desktop` (`2rem`) with `gutter-desktop` (`1.5rem`).
- **Compact Desktop / Field Terminals (1024px – 1279px):** 12 columns with condensed `gutter` (`1rem`) and `margin` (`1rem`).
- **Sidebars & Operational Trays:** Persistent vertical navigation docks (fixed at `240px` or icon-collapsed to `56px`) offset the main computational grid without causing layout shifts.

### Spacing Cadence
Spacing follows an exact 4px unit system. Components enforce internal pad constraints:
- Metric tiles: `space-md` (`1rem`) internal padding.
- Dense tabular rows: `space-sm` (`0.5rem`) vertical clearance.
- Module separation: `space-lg` (`1.5rem`) to maintain boundary legibility without wasting analytical space.

## Elevation & Depth

This design system deliberately minimizes soft or exaggerated drop shadows, which obscure boundary detection in high-precision desktop software. Depth is achieved via structural containment and disciplined surface hierarchy.

### Tonal Stratification & Borders
- **Level 0 (App Canvas):** `#F8FAF5` matte surface.
- **Level 1 (Module/Card):** `#FFFFFF` surface enclosed by a sharp `1px` border (`#DCE5D3`). This constitutes 90% of all content containers.
- **Level 2 (Active Panels / Flyouts):** `#FFFFFF` surface with a crisp structural hairline border (`#B7CD7F`) accompanied by an ambient, low-spread shadow: `0 4px 16px -2px rgba(15, 23, 16, 0.06)`.
- **Level 3 (Modal / Command Palette):** `#FFFFFF` surface framed by `#283E06` (1px) with an ambient shadow: `0 12px 32px -4px rgba(15, 23, 16, 0.12)`.

### Overlays & Insets
Recessed zones (such as live command inputs or payload log boxes) utilize `#F1F5EB` with an inset `1px` border (`#DCE5D3`), establishing clear visual wells for immediate data entry.

## Shapes

The design system uses a strict, low-radius shape system (`roundedness: 1`). Elements favor crisp, technical corners over friendly consumer curves.

### Corner Radii Definitions
- **Base Components (Inputs, Buttons, Badges, Tabs):** `0.25rem` (4px). Preserves the mechanical feeling of instrument panels.
- **Containers & Modules (`rounded-lg`):** `0.5rem` (8px). Used on analytical cards, complex charting widgets, and dialog surfaces.
- **Deep Windows (`rounded-xl`):** `0.75rem` (12px). Dedicated exclusively to global overlays, diagnostic viewports, and payload status modals.
- **Circular Elements:** Fully rounded shapes are strictly prohibited except for 6px status indicator pips and circular user node anchors.

## Components

### Buttons
- **Primary:** Solid `#283E06` fill, `#FFFFFF` text (JetBrains Mono, weight 500), `0.25rem` radius. Hover shifts to `#3B5110`. Focus ring exhibits a 2px offset in `#B7CD7F`.
- **Secondary / Outline:** Crisp `#FFFFFF` surface, 1px border (`#DCE5D3`), text `#0F1710`. Hover transitions border to `#283E06` and background to `#F1F5EB`.
- **Destructive / Abort:** Border `#D9381E` with subtle `#FDF3F2` background, text `#B32612`.

### Telemetry Badges & Status Chips
- Height constrained to `20px` or `24px` with `0.25rem` radius.
- **Nominal / Online:** Background `#F1F5EB`, 1px border `#B7CD7F`, text `#283E06`, leading with a solid 6px `#3B5110` dot.
- **Standby / Calibration:** Background `#F8FAF5`, 1px border `#DCE5D3`, text `#424B3E`.
- **Telemetry Readout Chip:** Background `#0F1710`, text `#B7CD7F`, monospaced tracking for live orbital frequencies and sensor IDs.

### Input Fields & Controls
- Height: `36px` compact baseline.
- **Background:** `#FFFFFF` resting, border 1px `#DCE5D3`.
- **Focus:** 1px `#283E06` border with a subtle 2px glow ring tinted in `#B7CD7F` at 40% opacity.
- **Prefix / Suffix:** Embedded unit badges (e.g., `hPa`, `km/s`, `ppm`) styled in `label-sm` color `#424B3E` within recessed `#F1F5EB` tabs.

### Selection Controls
- **Checkboxes:** `16px x 16px` square with `0.125rem` (2px) radius. Unchecked has a 1px `#DCE5D3` stroke; checked fills with `#283E06` displaying a `#FFFFFF` hairline tick.
- **Radio Buttons:** Concentric ring system. Selected state triggers a `#283E06` outer ring with a nested `#283E06` center pip against a white boundary.

### Cards & Telemetry Matrices
- Base background `#FFFFFF` enclosed with a 1px `#DCE5D3` stroke.
- Header bands are segregated using a hairline horizontal rule (`#DCE5D3`) with `space-sm` vertical padding.
- Sensor metric cells: Top label in `label-sm` (`#424B3E`), metric body in `telemetry-num` (`#0F1710`), and delta rate in `#3B5110` with contextual vector symbols.

### Additional Domain Components
- **Spectral / Value Trackers:** Horizontal distribution bars with `#F1F5EB` tracks and segmented fills in `#3B5110` and `#B7CD7F`.
- **Coordinate Breadcrumbs:** Monospaced terminal pathways (`ORBIT // SENSOR-04 // SPEC-B`) separated by forward slashes in `#B7CD7F`.