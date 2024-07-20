## Introduction

[MSYS2](https://www.msys2.org/) (Minimal SYStem 2) is a software distribution and a collection of tools for [[Windows]] that provide a Unix-like environment. It includes the GCC (GNU Compiler Collection) which is a compiler system produced by the GNU Project supporting various programming languages. This guide will walk you through the steps to install GCC using [MSYS2](https://www.msys2.org/) on a Windows system.
## Prerequisites

- [[Windows]] operating system.
- Administrator access to the system.
- An active internet connection.
## Step-by-Step Guide

### Step 1: Download MSYS2 Installer

1. Go to the [MSYS2 official website](https://www.msys2.org/).
2. Click on the "Download MSYS2" button.
3. Choose the appropriate installer for your system (64-bit or 32-bit).

### Step 2: Install MSYS2

1. Run the downloaded installer (e.g., `msys2-x86_64-20210725.exe`).
2. Follow the installation prompts and accept the default settings.
3. After installation, the [MSYS2](https://www.msys2.org/) terminal will open automatically. If it does not, you can open it from the Start Menu (search for "MSYS2 MSYS").

### Step 3: Update MSYS2

1. Open the [MSYS2](https://www.msys2.org/) MSYS terminal.
2. Update the package database and base packages by running the following commands:

```sh
pacman -Syu
```

3. Close the [MSYS2](https://www.msys2.org/) terminal and open it again.
4. Complete the update by running

```sh
pacman -Su
```

### Step 4: Install GCC

1. Open the [MSYS2](https://www.msys2.org/) MSYS terminal (if it’s not already open).
2. Install the base development tools which include GCC by running:
    
```sh
pacman -S --needed base-devel mingw-w64-x86_64-toolchain
```
- If you are using a 32-bit system, use the following command instead:

```sh
pacman -S --needed base-devel mingw-w64-i686-toolchain
```
### Step 5: Verify the Installation

1. To verify that GCC is installed, run:

```sh
gcc --version
```

2. You should see the version information for GCC if the installation was successful.

### Step 6: Set Environment Variables (Optional)

1. To make sure the [MSYS2](https://www.msys2.org/) tools are available from any command prompt, you might want to add the [MSYS2](https://www.msys2.org/) binary directories to your system PATH.
2. Open the System Properties window (Windows Key + Pause/Break, then click "Advanced system settings").
3. Click "Environment Variables".
4. In the "System variables" section, find the `Path` variable and click "Edit".
5. Add the following paths (assuming default installation paths) to the end of the variable value, separated by semicolons:

```shell
C:\msys64\mingw64\bin
C:\msys64\usr\bin
```

6. Click OK to close all the windows.
## Conclusion

You have successfully installed GCC with [MSYS2](https://www.msys2.org/) on your Windows system. You can now use GCC to compile your C/[[C++]] projects in a Unix-like environment on Windows. If you encounter any issues, refer to the [MSYS2](https://www.msys2.org/) [documentation](https://www.msys2.org/docs/) or seek help from the community.

---

This guide provides a step-by-step approach to installing GCC using MSYS2 on Windows, ensuring that users have a reliable and consistent development environment.