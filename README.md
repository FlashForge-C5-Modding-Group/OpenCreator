# OpenCreator

OpenCreator is the documentation home for the Creator 5 full custom firmware
project. The website source is in [`docs/`](docs/index.md) and is built with
MkDocs on Read the Docs. It covers architecture, current print workflow,
verification status, installation boundaries, and the stock-based Legacy path.

## Preview locally

```sh
python -m venv .venv
.venv/bin/python -m pip install -r docs/requirements.txt
.venv/bin/python -m mkdocs serve
```

On Windows, use `.venv\Scripts\python.exe` in place of `.venv/bin/python`.
Run `.venv/bin/python -m mkdocs build --strict` (or its Windows equivalent)
before submitting documentation changes.

## Publish on Read the Docs

Import `FlashForge-C5-Modding-Group/OpenCreator` in the Read the Docs
dashboard and select this repository's `main` branch. The root
`.readthedocs.yaml` installs the pinned documentation dependencies and builds
`mkdocs.yml`. Read the Docs will provide the project URL after import; no
deployment credentials belong in this repository.

The site is development documentation, not a flashing guide. Add a release
installation or recovery procedure only after it has been tested on the
specified hardware and build. Keep current source behavior separate from
verified printer behavior.
