## List all installed packages on an Ubuntu

To list all installed packages on an Ubuntu system, you can use the following command:

```bash
dpkg --list
```

Or, if you prefer a more detailed output:

```bash
dpkg -l
```

Alternatively, if you are using `apt` and want a simpler list:

```bash
apt list --installed
```

Each of these commands will display a list of installed packages, including version information.