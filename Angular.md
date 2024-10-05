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