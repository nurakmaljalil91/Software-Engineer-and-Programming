---
title: Micro frontend
category: microfrontend
tags: [#microfrontend]
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Overview

## Simple Implementation in Angular

### Create Host Application

Create new [[Angular]] 20 new project

```bash
ng new cerxos-shell
```

Use this options:

```bash
✔ Which stylesheet format would you like to use? CSS             [ https://developer.mozilla.org/docs/Web/CSS                     ]
✔ Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)? No
✔ Do you want to create a 'zoneless' application without zone.js? Yes
✔ Which AI tools do you want to configure with Angular best practices? https://angular.dev/ai/develop-with-ai None
```

Go to project folder

```bash
cd cerxos-shell
```

Add Native federation (host)

```bash
ng add @angular-architects/native-federation --project cerxos-shell --type dynamic-host --port 4200
```

### Create Remote Application

Create new [[Angular]] 20 new project

```bash
ng new planning-mfe
```

Use this options:

```bash
✔ Which stylesheet format would you like to use? CSS             [ https://developer.mozilla.org/docs/Web/CSS                     ]
✔ Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)? No
✔ Do you want to create a 'zoneless' application without zone.js? Yes
✔ Which AI tools do you want to configure with Angular best practices? https://angular.dev/ai/develop-with-ai None
```

Go to project folder

```bash
cd planning-mfe
```

Add Native federation (host)

```bash
ng add @angular-architects/native-federation --project planning-mfe --type remote --port 4201
```