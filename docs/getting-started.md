# Getting started

This page is a starting point for contributors and early testers. It is not
yet a printer installation guide.

## Explore the project

1. Read the [project layout](project-layout.md) to find the firmware,
   filesystem, and documentation repositories.
2. Check each repository's branch and README before building or flashing.
3. Use verified board status and recovery instructions once those are
   published here. Do not assume a development build is safe to flash.

## Preview this documentation

From the root of the OpenCreator repository:

```sh
python -m venv .venv
.venv/bin/python -m pip install -r docs/requirements.txt
.venv/bin/python -m mkdocs serve
```

On Windows, replace `.venv/bin/python` with `.venv\Scripts\python.exe`.
The preview server prints a local URL when it starts. Stop it with `Ctrl+C`.
