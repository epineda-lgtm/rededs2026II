# Repository Guidelines

## Project shape

- This is a standalone browser application in [`index.html`](../index.html). HTML, inline CSS, and application JavaScript are kept together; there is no separate source/build pipeline. Initial graph data is loaded from `Data/Points-ingredientes.csv` and `Data/Links-ingredientes.csv`.
- D3.js v7 and Tailwind CSS are loaded from CDNs in the page head. The app expects network access to load them.
- The data flow is CSV text -> `parseCustomCSV` -> `processGraphData` (which builds the shared node/link state, degree counts, neighbors, and categories) -> `getFilteredData` -> `renderCurrentMode`. Seven D3 renderers share the SVG interaction and drawing helpers.
- UI controls call globally scoped functions through inline HTML event handlers. Keep handler names and the corresponding control IDs/active-mode keys in sync when changing the interface.

## Data and rendering conventions

- The CSV parser is positional. Current defaults have node columns `Nodo,Categoría` and connection columns `Origen,Destino,Peso`; optional node columns are weight and coordinates, and optional connection columns are type and direction. The parser accepts comma- or semicolon-separated files, quoted fields, and a UTF-8 BOM. Keep generated templates and uploaded files consistent with those positions.
- Connection endpoints must match node names exactly, since each node's `name` is also its `id` and the key used in `state.nodeMap`.
- D3 force layouts mutate link endpoints from string IDs into node objects. Code that reads endpoints must handle either representation; the force renderer uses copies so the shared data remains reusable by other layouts.
- Keep graph data/state processing separate from view-specific layout calculations. Add a new visualization by registering its mode control/key and dispatch case, then implement its layout using the shared drawing helpers where appropriate.
- The UI labels and explanatory comments are primarily Spanish; retain Spanish for user-facing text and match the existing English naming style for JavaScript identifiers.

## Run and validate

- There are no package/build manifests, automated test runner, or lint configuration in this repository.
- Serve the repository root with a local static HTTP server and open `http://localhost:8000`; the default CSVs are fetched from `Data/`, so opening the HTML directly as a `file://` URL will not load them. Internet access is also needed for the CDN dependencies. No build step is required.
- No single-test command is defined. For a focused manual smoke check, load the page and exercise the changed visualization/control or CSV path, including a node and its matching connection if validating CSV processing.
