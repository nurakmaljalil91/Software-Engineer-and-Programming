---
title: Create virtual environment for Python
category: python
tags:
  - python
created: 2026-03-28
updated: 2026-03-28
status: active
---
- Make some directory and enter it
- Run this command to create the virtual environment in PowerShell

```powershell
PS > python -m venv venv
```

- To activate it, just run this command

```powershell
PS > venv/Scripts/activate
```

- To install the package, you can use `pip`

```powershell
(venv) PS > python -m pip install <package-name>
```
