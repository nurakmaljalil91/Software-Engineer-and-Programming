---
title: Flutter
category: flutter
tags:
  - flutter
  - dart
  - mobile
  - cross-platform
created: 2026-10-30
updated: 2026-10-30
status: active
---

## Overview

**Flutter** is an open-source UI software development kit created by Google. It is used to develop cross-platform applications from a single codebase for Android, iOS, [[Linux]], macOS, [[Windows]], and the web using the **Dart** programming language.

Unlike traditional wrappers or web-view frameworks, Flutter uses its own high-performance rendering engine (**Impeller** / **Skia**) to draw every pixel directly onto the screen, delivering native-level performance and consistent UI across platforms.

## Setup & Environment

- [[Setup Mobile App Development with Flutter and Android Studio]] — Complete guide to installing Flutter SDK, configuring Android Studio, Android SDK, and hardware-accelerated emulators.

## Core Architecture Concepts

- **Everything is a Widget**: UI elements, layout constraints, padding, and even the application root are widgets (`StatelessWidget` vs `StatefulWidget`).
- **Declarative UI**: UI is a function of state ($UI = f(state)$), similar to [[React]] and [[Vue]].
- **Hot Reload & Hot Restart**: Inject updated Dart source code files directly into the running Dart Virtual Machine (VM) without losing current application state.

## Related Notes

- [[Linux]]
- [[CachyOS]]
- [[Windows]]
- [[Visual Studio Code]]
- [[Clean Architecture]]
