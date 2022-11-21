Getting data in and out of locked down networks

## Python Simple Web Servers

* Python 2.7:

    ```
    python -m SimpleHTTPServer <optional_port>
    ```

* Python 3.x:

    ```
    python3 -m http.server <optional_port>
    ```

## PowerShell, downloading files

Download a file from a reachable URL via PowerShell (_TODO: Which PowerShell version required?_)

```
(New-Object system.net.webclient).downloadFile("<url>", "<dest_path>")
```