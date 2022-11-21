# Introduction

__Ideas__

1. Common wireshark plugins
1. How to filter
1. How to decode
1. How to decrypt SSL and TLS with the proper certificate

# Contents

[[_TOC_]]

# Useful Filters

## Ignore TCP retransmissions

Useful when performing a MITM attack to remove redundant traffic.

```
!(tcp.analysis.retransmission) and !(tcp.analysis.fast_retransmission)
```

## Searching frame contents (packet data search)

* Search in packet frame

    ```
    frame contains "<whatever>"
    ```

* Pearl compatible regex

    ```
    frame matches "<pearl_compatible_regex>"
    ```

* Case insensitive

    ```
    frame matches "(?i)<pearl_compatible_regex>"
    ```

* __Example:__ Case insensitive UTF-16 String (SQL query search)

    ```
    frame matches "(?i)s.e.l.e.c.t"
    ```