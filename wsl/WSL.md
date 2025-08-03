## Commands

Installing WSL using [[Windows]] Terminal

```powershell
wsl --install
```

Reboot the machine to let it install

List all the installed [[Linux]] distribution

```powershell
wsl --list
```

List all [[Linux]] distribution that can be install

```powershell
wsl --list --online
```

List all running [[Linux]] distribution

```powershell
wsl -l -v
```

Run the [[Linux]] distribution

```powershell
wsl -d ubuntu
```

Terminate one of the [[Linux]] distribution

```powershell
wsl --terminate ubuntu
```

Shutdown all [[Linux]] distributions

```powershell
wsl --shutdown
```

Uninstall one of the [[Linux]] distribution

```powershell
wsl --unregister ubuntu
```