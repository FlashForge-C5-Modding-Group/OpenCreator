# Contributing documentation

Edit pages under `docs/` and add new pages to `nav` in `mkdocs.yml`. Keep
hardware instructions tied to a specific printer model, firmware version,
source commit, and test result. Call out unverified behavior explicitly.

Make sure to use clear language and only delve into techincal specs and docs on parts
where it would be expected. 

Before proposing a change, build the site with warnings treated as errors:

```sh
.venv/bin/python -m mkdocs build --strict
```

On Windows, use `.venv\Scripts\python.exe`. The generated `site/` directory
is ignored by Git; only the source Markdown and configuration should be
committed.
