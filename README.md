# Digital Forensic Evidence Analyzer

A desktop and command-line tool for inventorying files in an evidence directory,
recording filesystem metadata and SHA-256 hashes, building a timestamp timeline,
and exporting investigation reports.

## Project structure

```text
.
├── src/
│   └── evidence_analyzer/
│       ├── __init__.py
│       ├── cli.py
│       ├── gui.py
│       ├── hex_viewer.py
│       ├── models.py
│       ├── reports.py
│       ├── scanner.py
│       └── timeline.py
├── tests/
│   └── test_analyzer.py
├── pyproject.toml
└── README.md
```

## Setup

Use Python 3.10 or later:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -e .
```

## Run

Launch the desktop interface:

```powershell
python -m evidence_analyzer --gui
```

Or, after installing the package, run `evidence-analyzer-gui`.

The GUI lets you choose the evidence and report folders, displays the file
inventory and timestamp timeline, and includes a bounded, read-only hex viewer.
Select an inventory row to inspect that file, or choose another file directly
in the hex viewer. The timeline provides event counts, event details, and a
current-scan snapshot; its count chart supports linear, square-root, and
logarithmic scales. History lists completed scans for the current app session
and lets you reopen their results. Reports are generated as with the CLI.

Run from the command line:

```powershell
evidence-analyzer "C:\path\to\evidence" --output "C:\path\to\reports"
```

Or, from the project root:

```powershell
python -m evidence_analyzer "C:\path\to\evidence" --output ".\reports"
```

The output directory contains:

- `evidence_inventory.csv` — file paths, sizes, timestamps, and hashes.
- `investigation_timeline.csv` — available filesystem timestamp events in UTC order.
- `forensic_summary.json` — scan totals and any files that could not be fully read.

The tool skips symbolic links and does not change file contents. Reading files
to calculate hashes can update access times depending on the operating system
and filesystem settings; use a read-only evidence image or mount when preserving
source metadata is required. Timestamps are filesystem metadata, not proof of
when a file was created or modified. Creation timestamps are included only on
platforms that expose them (or on Windows, where the creation-time field is
available).

## Test

```powershell
python -m unittest discover -s tests -v
```
