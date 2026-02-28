# home-lab-nmap-enumeration
Practical network scanning and service analysis in a controlled home lab environment for cybersecurity learning.


1) SYN Scan (-sS) (nmap sS scan.png)
   ---------------------------------
   This screenshot demonstrates a TCP SYN scan performed using
       nmap -sS <target ip>
   The SYN scan is also known as a "half-open" or stealth scan. Instead of completing the full TCP three-way handshake, Nmap sends a SYN packet and analyzes the      response.

   HOW IT WORKS:
   --------------
   If the target replies with SYN-ACK -> the port is OPEN
   If the target replies with RST -> the port is CLOSED
   If there is no response or filtered response -> the port is FILTERED

   This scan is faster and less detectable than a full TCP connect scan becuase it does not complete the connection.

2) TCP Connect Scan (-sT) (nmap -sT scan.png)
   ------------------------------------------
   This screenshot demonstrates a full TCP Scan performed using
    nmap -sT <target ip>
   The TCP connect scan is a full connection scan.
   Instead of stopping halfway like a SYN scan, Nmap completes the entire TCP three-way handshake. It uses the operating system's normal connect function to          establish a real connection

   HOW IT WORKS
   ------------
   If the target replies with SYN-ACK -> Nmap sends ACK -> the port is OPEN (connection fully established)
   If the target replies with RST -> the port is CLOSED
   If there is no response or an ICMP unreachable message -> the port is FILTERED

   Unlike the SYN scan, this method fully establishes the connection before closing it.

3)  No Ping Scan (-Pn) (nmap -Pn scan.png)
    --------------------------------------
    This screenshot demonstrates No ping Scan
    nmap -Pn <target ip>
    The -Pn option tells Nmap to skip host discovery and assume the target host is online.
    Normally, Nmap first sends ping probes (ICMP, TCP ACK, etc.) to check if the host is alive.
    with -Pn, this step is skipped.

    HOW IT WORKS
    --------------
   Nmap does not send ping requests before scanning
   It assumes the host is UP
   It proceeds directly to port scanning

   If ports respond normally -> Nmap shows OPEN or CLOSED
   If firewall blocks probes -> Ports may appear FILTERED
   If host is actually down -> Scan will still run but no ports respond

 4) Full Port Scan (-p-) ( nmap -p- scan.png)
    -----------------------------------------
    This screenshot demonstrates Full Port Scan
       nmap -p- <target ip>
    The -p- option in Nmap to scan all 65,535 TCP ports instead of only the default top 1000 ports
    By default, Nmap scans the most common ports.
    with -p-, it scans the entire TCP port range (1-65535).

     HOW IT WORKS
     ------------
     Nmap sends probes to every TCP port from 1 to 65535
     If a port replies with SYN-ACK -> the port is OPEN
     If a port replies with RST -> the port is CLOSED
     If there is no response or filtered response -> the port is FILTERED

    This  option useful when 
    a)Services are running on uncommon ports
    b)You want a complete attack surface analysis
    c)Performing thorough reconaissance

5)  Service Version Detection(-sV) and OS detection (-O) (nmap -sS -sV -O scan.png)
    -----------------------------------------------------   
   -sV
   ----
   The -sV option in Nmap enable services and version detection on open ports.
   After identifying open ports, Nmap sends additional probes to determine wich service is running and its version

  HOW IT WORKS
  -------------
  Nmap first identifiies open ports
  It sends specialized probes to those ports
  It analyzes the response banner or behaviour
  It matches responses against its service database
  It reveals Service name (HTTP,SSH), Software version (OpenSSH 8.2, Apache 2.4.41) and some additional service details

  It Useful when identifying outdated software, checking for known vulnerabilities and performing vulnerability assessment

  -O
  ---
  The -O option enables Operating System detection.
  It attempts to determine target system's OS by analyzing TCP/IP stack behaviour.

  HOW IT WORKS
  -------------
  Nmap sends specially crafted packets
  It analyzes how the target responds
  It compares the response pattern with its OS fingerprint database
  It reveals likely operating system(Linux, Windows, etc.), Possible OS version and TCP/IP stack characteristics

  It useful when mapping network environments, Identifying target platforms and Understanding attack surface.
  

