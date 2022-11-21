# Avoiding Detection

There are several techniques to avoid detection. When running an Nmap scan for example, specifying `-T4` will result in a very loud scan while specifying `-T2` or `-T1` will be much quieter (also much slower). Avoiding binary detection is also important. If you drop known malware or an exploit toolkit (like meterpreter), there are several techniques to avoid detection (none guaranteed).

## Packing Executables

### UPX

```
upx --best --brute <exe> -o <out_exe>
```