## Install a "Nerd Font"

- Install `Nerd Font`, go to [Nerd Font Website](https://www.nerdfonts.com/font-downloads)
- Download `JetBrains Mono Nerd Font`
- Unzip the file and select all ending in `.tff` or `.otf` and choose **install**
## Install "Oh My Posh"

- Open Windows Terminal (PowerShell) and run this command

```bash
winget install JanDeDobbeleer.OhMyPosh -s winget
```

- Close and restart the terminal

## Activate the Theme

- In PowerShell type the following command

```shell
notepad $PROFILE
```

or 

```bash
code $PROFILE
```

- Copy this to the editor and save

```shell
oh-my-posh init pwsh | Invoke-Expression
```

## Configure Windows Terminal to use the Font

- In Windows Terminal, press `Ctrl + , (comma) to open Settings
- On the left sidebar, click on **Windows PowerShell** (under "Profiles").
- Click on the **Appearance** tab.
- Find the **Font face** dropdown and change it to **JetBrains Mono Nerd Font** (or whichever Nerd Font you installed).
- Click **Save**.

## Add File Icons

- Install icon module by running this command

```shell
Install-Module -Name Terminal-Icons -Repository PSGallery
```

- Open the profile again

```shell
notepad $PROFILE
```

or 

```bash
code $PROFILE
```

- Add this line **below** the Oh My Posh line you added earlier:

```shell
Import-Module -Name Terminal-Icons
```

## Installing Theme

- Run this command to create a  themes folder in user directory

```bash
New-Item -Path $HOME\.poshthemes -ItemType Directory -Force
```

- Download the Atomic theme file manually to the folder

```bash
Invoke-WebRequest -Uri 'https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/atomic.omp.json' -OutFile "$HOME\.poshthemes\atomic.omp.json"
```

- Update profile to point to this new local file

```bash
oh-my-posh init pwsh --config "$HOME\.poshthemes\atomic.omp.json" | Invoke-Expression
```