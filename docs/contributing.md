# Contributing documentation

Edit pages under `docs/` and add new pages to `nav` in `mkdocs.yml`. For
hardware instructions, say which printer model, firmware version and source
commit they're for, and how they were tested. Say plainly when something
hasn't been tested.

Make sure to use clear language and only delve into techincal specs and docs on parts
where it would be expected. 

Before proposing a change, build the site with warnings treated as errors:

```sh
.venv/bin/python -m mkdocs build --strict
```

On Windows, use `.venv\Scripts\python.exe`. The generated `site/` directory
is ignored by Git; only the source Markdown and configuration should be
committed.
