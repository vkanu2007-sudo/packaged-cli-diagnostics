# Packaged CLI Diagnostics Tool

A small, installable Python command-line application that checks a developer
machine and creates a deterministic environment health report.

## Features

- Checks the running Python version against a configurable minimum.
- Checks free disk space at a configured path.
- Checks whether configured environment variables are present (values are
  never printed or stored in the report).
- Checks whether configured developer tools are available on `PATH`.
- Prints a human-readable report or structured JSON.
- Uses stable report fields and does not add a generated timestamp.

## Requirements

- Python 3.9 or newer
- No third-party runtime dependencies

## Install

Create and activate a virtual environment:

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS / Linux:
source .venv/bin/activate
```

Install the project in editable mode:

```bash
python -m pip install -e .
```

## Run

Run the default diagnostics:

```bash
env-health
```

Print JSON:

```bash
env-health --format json
```

Save a report:

```bash
env-health --format json --output report.json
env-health --format text --output report.txt
```

Use a custom configuration:

```bash
env-health --config config.example.json
```

Combine options:

```bash
env-health --config config.example.json --format json --output report.json
```

## Configuration

The configuration file is JSON. All fields are optional.

```json
{
  "min_python": "3.9",
  "disk_path": ".",
  "required_env": ["PATH"],
  "required_tools": ["python", "git"]
}
```

- `min_python`: minimum Python version, in `major.minor` form.
- `disk_path`: directory or mount point whose free space will be checked.
- `required_env`: environment variable names that must exist and be non-empty.
- `required_tools`: executable names that must be discoverable on `PATH`.

A missing or malformed configuration file is a configuration error.

## Exit codes

| Code | Meaning |
| --- | --- |
| `0` | All checks passed |
| `1` | Diagnostics ran, but one or more checks failed |
| `2` | Configuration or execution error |

## Tests

Run the unit tests from the repository root:

```bash
python -m unittest discover -s tests -v
```

## Project structure

```text
packaged-cli-diagnostics/
├── pyproject.toml
├── README.md
├── config.example.json
├── samples/
│   ├── healthy-report.json
│   └── healthy-report.txt
├── src/
│   └── envdoctor/
│       ├── __init__.py
│       ├── __main__.py
│       ├── cli.py
│       ├── diagnostics.py
│       └── reporting.py
└── tests/
    └── test_diagnostics.py
```
