# Amazon Connect Log Viewer

> A zero-install, single-file browser tool for exploring Amazon Connect Contact Flow logs exported as CSV.

**No server. No install. One file. Open in browser and go.**

---

## Overview

Upload an Amazon Connect CSV log export and interactively filter, explore, and inspect flow executions. Focused on Lambda block inspection (parameters + responses), with a visual call flow diagram showing the path through flows and modules.

---

## Tech Stack

| Concern      | Choice                  |
|--------------|-------------------------|
| Structure    | Plain HTML5             |
| Styling      | Pico.css (CDN)          |
| CSV Parsing  | PapaParse (CDN)         |
| Logic/UI     | Vanilla JS (ES6+)       |
| Diagrams     | Native SVG              |

**Total external dependencies: 2 CDN links.**

---

## Project Structure

```
amazon-connect-log-viewer/
├── index.html    ← everything (HTML + CSS + JS inline)
├── sample-data/  ← example CSV files for testing
└── README.md
```

---

## Expected CSV Format

The CSV has **2 columns**: `timestamp` (epoch millis) and `message` (a JSON string).

Each `message` JSON contains:

| Field | Description |
|---|---|
| `ContactId` | Unique call identifier |
| `ContactFlowName` | Flow/module name |
| `ContactFlowModuleType` | Block type (e.g. `InvokeExternalResource`, `SetAttributes`) |
| `Identifier` | Block name or UUID |
| `Timestamp` | ISO 8601 timestamp |
| `Parameters` | Block input parameters (object) |
| `ExternalResults` | Lambda/external call response (object, when present) |
| `Results` | Branch taken for check blocks (e.g. `true`/`false`) |
| `ContactFlowId` | ARN of the flow/module |

---

## Features

### File Upload
- Drag-and-drop or click-to-browse
- Parsed in-browser — no data leaves the machine
- Shows filename, row count, and ContactId after upload

### Filters
- **Flow Name** dropdown — filter by `ContactFlowName`
- **Block Type** dropdown — filter by `ContactFlowModuleType`
- **Quick filter buttons** — one-click filters for common block types: Lambda, Prompt, SetAttr, CheckAttr, FlowModule
- **Free-text search** — searches all fields in every row

### Results Table

Columns: `#` · `Flow Name` · `Block Type` · `Lambda` · `Identifier` · `Timestamp`

- Sorted by Timestamp ascending by default
- Row count shown as `X of Y`
- Capped at 500 rows for performance
- **Lambda rows** — highlighted with a blue left border and `Lambda` badge; the `Lambda` column shows the function name extracted from the ARN
- **Error rows** — highlighted red with an `Error` badge when `ExternalResults` contains errors
- **Grouped SetAttributes** — consecutive SetAttributes blocks with the same flow/identifier are collapsed into one row with a count badge (e.g. `3x`)
- **CheckAttribute rows** — show a `true`/`false` badge inline
- **Expand All / Collapse All** — toggle detail panels for all rows at once

### Detail Panel (inline expand)

Click any row to expand it in place. Closes by clicking again.

**Lambda rows show:**
- Parameters (pretty-printed JSON)
- External Results (pretty-printed JSON, labeled if errors present)
- Block Info (flow, type, identifier, timestamp, flow ID)

**Other rows show:**
- Block Info (including `Result` branch for CheckAttribute)
- Parameters (if present)

All rows include a collapsible **Raw Row** section with the full JSON.

### Call Flow Diagram

Click **Call Flow** (appears after upload) to open an SVG diagram showing the sequence of flows/modules the contact passed through. Each node shows the flow name, whether it's a flow or module, and how many blocks executed in it. Nodes are connected with arrows in a serpentine layout. Close with `✕` or `Esc`.

---

## How to Use

1. Open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari)
2. Drop or select your Amazon Connect CSV export
3. Use the dropdowns, quick-filter buttons, or search bar to narrow results
4. Click any row to inspect block details and Lambda parameters/responses
5. Click **Call Flow** to see the flow path as a diagram

**No internet required after first load** (CDN links cache automatically).

---

## Known Limitations

- **Browser only** — no backend
- **No persistence** — data resets on page refresh (intentional for privacy)
- **Single file at a time** — no multi-CSV merge
- **Capped at 500 rendered rows** — for performance; upload summary shows total
