--------
# DOMAINS

1. Domain Name is the address of our website that we type in the URL bar of browser.
2. Earlier when we wanted to access any website, we had to type its IP address which is very difficult to remember. So domain name solved this problem.


-----------------------
## <u>Working of Domains</u>

Refer to the picture below:

![working_domains](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/working_of_domains.PNG)
<p align="center">Fig B1: Working of domain</p>

1. When we enter a domain name in our web browser, it first sends a request to a global network of servers that form the Domain Name System (DNS).
2. These servers then look up for the name servers associated with the domain and forward the request to those name servers.
3. <b><u>Eg:</u></b> if our website is hosted on Bluehost, then its name server information will be like this:

ns1.bluehost.com

ns2.bluehost.com

4. These name servers are computers managed by your hosting company. 
5. Our hosting company will forward your request to the computer where our website is stored.
6. This computer is called a ***web server***. It has special software installed (Apache, Nginx are two popular web server software). The web server now fetches the web page and pieces of information associated with it.
7. Finally, it then sends this data back to the browser.

-----------------
## <u>DNS Records</u>

1. DNS Records is the smallest entity that stores the precise information about which domain name belongs to which target address.
2. Below is a picture of how the entry looks like:

![dns_record](https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/blob/main/Page_images/dns_record.PNG)
<p align="center">Fig B2: DNS records</p>

***NOTE:*** 📕

***TTL indicates how long (in seconds) a name resolution that has just taken place is likely to remain valid.***


----------------------------------------
## <u>Commonly used types of DNS Records</u>

|  Type   |   Description    |
|---------|------------------|
|  A (IPv4 Host address) |  <ul><li>Most basic and the most commonly used DNS record type</li><li>Used to translate human friendly domain names such as "www.example.com" into IP-addresses such as 23.211.43.53 (machine friendly numbers).</li><li>A-records are the DNS server equivalent of the hosts file - a simple domain name to IP-address mapping.</li><li>This record type is defined in RFC1035</li></ul>  | 
|  AAAA (IPv6 host address)  | <ul><li>AAAA-record is used to specify the IPv6 address for a host (equivalent of the A-record type for IPv4)</li><li>IPv6 is the new replacement for the old IPv4 address system.</li><li>This record type is defined in RFC3596.</li></ul> | 
|  CNAME (Canonical name for an alias)  |  <ul><li>CNAME-records are domain name aliases.</li><li>Computers on the Internet often performs multiple roles such as web-server, ftp-server, chat-server etc.To mask this, CNAME-records can be used to give a single computer multiple names (aliases).</li><li>***Eg:*** the computer "computer1.xyz.com" may be both a web-server and an ftp-server, so two CNAME-records are defined: "www.xyz.com" = "computer1.xyz.com" and "ftp.xyz.com" = "computer1.xyz.com".<p>Sometimes a single server computer hosts many different domain names (take ISPs), and so CNAME-records may be defined such as "www.abc.com" = "www.xyz.com".</p></li><li>The most common use of the CNAME-record type is to provide access to a web-server using both the standard "www.domain.com" and "domain.com" (with and without the www prefix). <p>This is usually done by creating an A-record for the short name (without www), and a CNAME-record for the www name pointing to the short name.</p></li><li>CNAME-records can also be used when a computer or service needs to be renamed, to temporarily allow access through both the old and new name.</li><li>A CNAME-record should always point to an A-record and never to itself or another CNAME-record to avoid circular references.</li></ul>  | 
|  MX (Mail eXchange)     |  <ul><li>MX-records are used to specify the e-mail server(s) responsible for a domain name.</li><li>Each MX-record points to the name of an e-mail server and holds a preference number for that server.</li><li>If a domain name is handled by multiple e-mail servers (for backup/redundancy), a separate MX-record is used for each e-mail server, and the preference numbers then determine in which order (lower numbers first) these servers should be used by other e-mail servers.</li><li>If a domain name is handled by a single e-mail server, only one MX-record is needed and the preference number does not matter.</li><li>When sending an e-mail to "user@example.com", your e-mail server must first look up any MX-records for "example.com" to see which e-mail servers handles incoming e-mail for "example.com".</li></ul>  | 
|  NS (Name Server)     |  <ul><li>NS-records identify the DNS servers responsible (authoritative) for a zone.</li><li>A zone should contain one NS-record for each of its own DNS servers (primary and secondaries).</li><li>This is mostly used for zone transfer purposes (notify messages).</li><li>These NS-records have the same name as the zone in which they are located.</li><li>An NS-record identifies the name of a DNS server - not the IP-address. <p>Because of this, it is important that an A-record and/or AAAA-record for the referenced DNS server exists (not necessarily on your DNS server, but wherever it belongs), otherwise there may not be any way to connect with that DNS server.</p></li></ul>    |  
|  PTR (Pointer)     |  <ul><li>PTR-records are primarily used as "reverse records" - to map IP addresses to domain names (reverse of A-records and AAAA-records).</li></ul>  | 
|  SOA (Start Of Authority)     |  <ul><li>Stores important information about a domain or zone such as the email address of the administrator, when the domain was last updated, and how long the server should wait between refreshes.</li></ul>   |      
|  SRV (location of service)     |  <ul><li>SRV-records are used to specify the location of a service.</li><li>They are used in connection with different directory servers such as LDAP (Lightweight Directory Access Protocol), and Windows Active Directory, and more recently with SIP servers</li></ul> |
|  TXT (Descriptive text)     |  <ul><li>TXT-records are used to hold descriptive text.</li><li>They are often used to hold general information about a domain name such as who is hosting it, contact person, phone numbers, etc.</li><li>One common use of TXT-records is for SPF</li></ul>   | 


------------
# NAMESERVERS

1. They manage the information about which IP addresses belong to which domains.


-------------------------------------------------------------------------------------------------------------
