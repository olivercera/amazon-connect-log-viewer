# Amazon Connect Log Viewer

## Purpose

A zero-install, single-file browser tool for exploring Amazon Connect Contact
Flow logs exported as CSV. Upload a CSV export and interactively filter,
explore, and inspect flow executions — with a focus on Lambda block
inspection (parameters + responses) and a visual call flow diagram showing
the path through flows and modules. Everything runs client-side; no data
ever leaves the browser.

Everything (HTML, CSS, JS) lives in a single file: [index.html](index.html).
See [README.md](README.md) for full feature docs, expected CSV format, and
usage instructions.

## Rule: bump the build version on every change

`index.html` defines a `BUILD_VERSION` constant that's rendered as a badge
in the UI:

```js
const BUILD_VERSION = '2026-09-16 14:32';
```

Whenever you make any change to `index.html`, update `BUILD_VERSION` to the
current date/time (`YYYY-MM-DD HH:MM`) as the last edit before finishing.
This is the only versioning mechanism in this project (no package.json,
no git tags) — the badge is how the user confirms they're looking at the
latest build after reloading the page.
