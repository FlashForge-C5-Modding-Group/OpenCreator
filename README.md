# OpenCreator

OpenCreator is the documentation home for the Creator 5 full custom firmware
project. The website source is in [`docs/`](docs/index.md) and is built with
[Zensical](https://zensical.org/) on Read the Docs. It covers architecture, current print workflow,
verification status, installation boundaries, and the stock-based Legacy path.

## Preview locally

```sh
python -m venv .venv
.venv/bin/python -m pip install -r docs/requirements.txt
.venv/bin/python -m zensical serve
```

On Windows, use `.venv\Scripts\python.exe` in place of `.venv/bin/python`.
Run `.venv/bin/python -m zensical build --strict` (or its Windows equivalent)
before submitting documentation changes.

## Publishing

The site is served at https://opencreatorcfw.github.io/. Every push to `main`
runs `.github/workflows/docs.yml`, which builds with Zensical and pushes the
output to `OpenCreatorCFW/opencreatorcfw.github.io` (branch `main`, which
GitHub Pages serves). The workflow authenticates with the `PAGES_DEPLOY_KEY`
secret, the private half of a write deploy key on that repository.
