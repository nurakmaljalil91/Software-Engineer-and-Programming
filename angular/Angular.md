---
title: Angular
category: angular
tags: [#angular]
created: 2026-03-28
updated: 2026-03-28
status: active
---
## Overview

Angular is a web framework that empowers developers to build fast, reliable applications.
## Setup a new project locally

If you're starting a new project, you'll most likely want to create a local project so that you can use tooling such as Git.
### Prerequisites

- **Node.js** - v[^18.19.1 or newer](https://angular.dev/reference/versions)
- **Text editor** - We recommend [Visual Studio Code](https://code.visualstudio.com/)
- **Terminal** - Required for running Angular CLI commands

### Instructions

The following guide will walk you through setting up a local Angular project.

#### Install Angular CLI

Open a terminal (if you're using [Visual Studio Code](https://code.visualstudio.com/), you can open an [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)) and run the following command:

```bash
npm install -g @angular/cli
```


If you are having issues running this command in Windows or Unix, check out the [CLI docs](https://angular.dev/tools/cli/setup-local#install-the-angular-cli) for more info.

#### Create a new project

In your terminal, run the CLI command `ng new` with the desired project name. In the following examples, we'll be using the example project name of `my-first-angular-app`.

```bash
ng new <project-name>
```


You will be presented with some configuration options for your project. Use the arrow and enter keys to navigate and select which options you desire.

If you don't have any preferences, just hit the enter key to take the default options and continue with the setup.

After you select the configuration options and the CLI runs through the setup, you should see the following message:

```bash
✔ Packages installed successfully. Successfully initialized git.
```

check

At this point, you're now ready to run your project locally!

#### Running your new project locally

In your terminal, switch to your new Angular project.

```bash
cd my-first-angular-app
```

All of your dependencies should be installed at this point (which you can verify by checking for the existent for a `node_modules` folder in your project), so you can start your project by running the command:

```bash
npm start
```

If everything is successful, you should see a similar confirmation message in your terminal:

```bash
Watch mode enabled. Watching for file changes...NOTE: Raw file sizes do not reflect development server per-request transformations. ➜ Local: http://localhost:4200/ ➜ press h + enter to show help
```

And now you can visit the path in `Local` (e.g., `http://localhost:4200`) to see your application.

## Angular Best Practices

Generate environments

```bash
ng generate environments
```

Add [[Tailwind CSS]]

```bash
npm install tailwindcss @tailwindcss/postcss postcss --force
```

Create a `.postcssrc.json` file in the root of your project and add the `@tailwindcss/postcss` plugin to your PostCSS configuration.


```powershell
type nul > .postcssrc.json
```


```json
{
  "plugins": {
    "@tailwindcss/postcss": {}
  }
}
```

Add an `@import` to `./src/styles.css` that imports Tailwind CSS.

```css
@import "tailwindcss";
```

Set up [[Prettier]] & [[ESLint]]

```bash
npm i -D prettier eslint
```

For the two tools to work together smoothly, install these additional packages:

```bash
npm i -D eslint-config-prettier eslint-plugin-prettier
```

add the _Angular ESLint_ plugins (including TypeScript support):

```bash
ng add angular-eslint
```

Copy this rules

```js
// eslint.config.js
// @ts-check
const eslint = require('@eslint/js');
const tseslint = require('typescript-eslint');
const angular = require('angular-eslint');
const eslintConfigPrettier = require('eslint-config-prettier');

module.exports = tseslint.config(
  {
    ignores: ['.angular/**', '.nx/**', 'coverage/**', 'dist/**'],
    files: ['**/*.ts'],
    extends: [
      eslint.configs.recommended,
      ...tseslint.configs.recommended,
      ...tseslint.configs.stylistic,
      ...angular.configs.tsRecommended,
      eslintConfigPrettier,
    ],
    processor: angular.processInlineTemplates,
    rules: {
      '@angular-eslint/directive-selector': [
        'error',
        {
          type: 'attribute',
          prefix: 'app',
          style: 'camelCase',
        },
      ],
      '@angular-eslint/component-selector': [
        'error',
        {
          type: ['attribute', 'element'],
          prefix: 'app',
          style: 'kebab-case',
        },
      ],

      // Angular best practices
      '@angular-eslint/no-empty-lifecycle-method': 'warn',
      '@angular-eslint/prefer-on-push-component-change-detection': 'warn',
      '@angular-eslint/prefer-output-readonly': 'warn',
      '@angular-eslint/prefer-signals': 'warn',
      '@angular-eslint/prefer-standalone': 'warn',

      // TypeScript best practices
      '@typescript-eslint/array-type': ['warn'],
      '@typescript-eslint/consistent-indexed-object-style': 'off',
      '@typescript-eslint/consistent-type-assertions': 'warn',
      '@typescript-eslint/consistent-type-definitions': ['warn', 'type'],
      '@typescript-eslint/explicit-function-return-type': 'error',
      '@typescript-eslint/explicit-member-accessibility': [
        'error',
        {
          accessibility: 'no-public',
        },
      ],
      '@typescript-eslint/naming-convention': [
        'warn',
        {
          selector: 'variable',
          format: ['camelCase', 'UPPER_CASE', 'PascalCase'],
        },
      ],
      '@typescript-eslint/no-empty-function': 'warn',
      '@typescript-eslint/no-empty-interface': 'error',
      '@typescript-eslint/no-explicit-any': 'warn',
      '@typescript-eslint/no-inferrable-types': 'warn',
      '@typescript-eslint/no-shadow': 'warn',
      '@typescript-eslint/no-unused-vars': 'warn',

      // JavaScript best practices
      eqeqeq: 'error',
      complexity: ['error', 20],
      curly: 'error',
      'guard-for-in': 'error',
      'max-classes-per-file': ['error', 1],
      'max-len': [
        'warn',
        {
          code: 120,
          comments: 160,
        },
      ],
      'max-lines': ['error', 400], // my favorite rule to keep files small
      'no-bitwise': 'error',
      'no-console': 'off',
      'no-new-wrappers': 'error',
      'no-useless-concat': 'error',
      'no-var': 'error',
      'no-restricted-syntax': 'off',
      'no-shadow': 'error',
      'one-var': ['error', 'never'],
      'prefer-arrow-callback': 'error',
      'prefer-const': 'error',
      'sort-imports': [
        'error',
        {
          ignoreCase: true,
          ignoreDeclarationSort: true,
          allowSeparatedGroups: true,
        },
      ],

      // Security
      'no-eval': 'error',
      'no-implied-eval': 'error',
    },
  },
  {
    files: ['**/*.html'],
    extends: [...angular.configs.templateRecommended, ...angular.configs.templateAccessibility],
    rules: {
      // Angular template best practices
      '@angular-eslint/template/attributes-order': [
        'error',
        {
          alphabetical: true,
          order: [
            'STRUCTURAL_DIRECTIVE', // deprecated, use @if and @for instead
            'TEMPLATE_REFERENCE', // e.g. `<input #inputRef>`
            'ATTRIBUTE_BINDING', // e.g. `<input required>`, `id="3"`
            'INPUT_BINDING', // e.g. `[id]="3"`, `[attr.colspan]="colspan"`,
            'TWO_WAY_BINDING', // e.g. `[(id)]="id"`,
            'OUTPUT_BINDING', // e.g. `(idChange)="handleChange()"`,
          ],
        },
      ],
      '@angular-eslint/template/button-has-type': 'warn',
      '@angular-eslint/template/cyclomatic-complexity': ['warn', { maxComplexity: 10 }],
      '@angular-eslint/template/eqeqeq': 'error',
      '@angular-eslint/template/prefer-control-flow': 'error',
      '@angular-eslint/template/prefer-ngsrc': 'warn',
      '@angular-eslint/template/prefer-self-closing-tags': 'warn',
      '@angular-eslint/template/use-track-by-function': 'warn',
    },
  },
);
```

Optional using .mjs

```js
// eslint.config.mjs
import js from '@eslint/js';
import * as tseslint from 'typescript-eslint';
import angular from 'angular-eslint';
import eslintConfigPrettier from 'eslint-config-prettier';

export default [
  // Global ignores (also exclude the root HTML document)
  { ignores: ['dist', 'coverage', 'node_modules', '.angular', 'src/index.html'] },

  // JS rules (apply only to JS files)
  {
    files: ['**/*.{js,cjs,mjs}'],
    ...js.configs.recommended,
    rules: {}
  },

  // Angular TS base recommendations (keep as their own entries)
  ...angular.configs.tsRecommended,

  // TypeScript: set parser + typed project + inline-template processing
  {
    files: ['**/*.ts'],
    languageOptions: {
      parser: tseslint.parser,
      parserOptions: {
        project: ['./tsconfig.eslint.json'],
        tsconfigRootDir: process.cwd()
      }
    },
    plugins: { '@typescript-eslint': tseslint.plugin },
    processor: angular.processInlineTemplates,
    rules: {}
  },

  // TypeScript rules — SCOPED to .ts (do not apply at top-level)
  // We remap the presets so they only run on TS files.
  ...tseslint.configs.recommended.map((c) => ({ ...c, files: ['**/*.ts'] })),
  ...tseslint.configs.recommendedTypeChecked.map((c) => ({ ...c, files: ['**/*.ts'] })),
  ...tseslint.configs.stylisticTypeChecked.map((c) => ({ ...c, files: ['**/*.ts'] })),

  // Your Angular selector prefs (TS only)
  {
    files: ['**/*.ts'],
    rules: {
      '@angular-eslint/directive-selector': [
        'error',
        { type: 'attribute', prefix: 'app', style: 'camelCase' }
      ],
      '@angular-eslint/component-selector': [
        'error',
        { type: 'element', prefix: 'app', style: 'kebab-case' }
      ]
    }
  },

  // Angular template rules — scope only to component templates
  ...angular.configs.templateRecommended.map((c) => ({
    ...c,
    files: ['src/app/**/*.html']
  })),
  ...angular.configs.templateAccessibility.map((c) => ({
    ...c,
    files: ['src/app/**/*.html']
  })),

  // Disable ESLint rules that clash with Prettier
  eslintConfigPrettier
];
```