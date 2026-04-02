---
title: Manage multiple Python Version
category: python
tags:
  - python
created: 2026-03-28
updated: 2026-03-28
status: active
---
- Install [Python Install Manager](https://www.python.org/downloads/)
- Install: `py install <version>` (e.g., `py install 3.14`).
- List Versions: `py list --online` (available) or `py list` (installed).
- Uninstall: `py uninstall <version>`.
- Launch: Use `py` or `pymanager` to start Python.
-  Navigate to your project's root directory and run `pyenv local <version>`. This creates a `.python-version` file that automatically activates that version when you are in that directory.