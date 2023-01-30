# **TryHackMe's Advent of Cyber 2022**

<br>

## An introduction to Red and Blue Teaming.
<br>

I recently participated in TryHackMe's annual Advent of Cyber event to improve my cybersecurity knowledge. Every year, they organize an amazing community-led event that teaches various penetration testing techniques, programs, and simulations. In this article, I'll explain what I learned each day of the event to help me compartmentalize the large amount of information I learned in December.

<br>

<br>

<br>

## **Day 1: Security Frameworks** 

<br>

Day one was mainly about Security Frameworks, which was an excellent introduction to how companies deal with the many vulnerabilities that exist in today's world. They covered the NIST framework, which focuses on the steps of Identifying, Protecting, Detecting, Responding, and Recovering from cyber-attacks. They also discussed the ISO 27000 Series, such as implementing and managing an information security management system (ISMS), and the MITRE ATT&CK Framework, which expands on Tactics, Techniques, and Procedures (TTPs). The Cyber Kill Chain and Unified Kill Chain (UKC) was also introduced to understand how a hacker gains access to a system or networked environment. This included Reconnaissance, Weaponization, Delivery, Social Engineering, Exploitation, Persistence, Defense Evasion, and Command & Control. Although the actual task was just a definition puzzle, it was a great introduction to real-world frameworks.

<br>

<br>

## **Day 2: Parsing Log Files** 

<br>

Day two focused on parsing log files to find meaningful information. This was very hands-on, as it showed how log files are stored in a Linux machine and how to search for the logs you want using commands like the following:


```
cd var/log
ls -lah
grep -i “tryhackme” log.txt
```


Through the activity, I found the associated log files for the day of an intrusion, the IP address of the attacker, and what the attacker stole.

<br>

<br>

## **Day 3: OSINT Techniques** 

<br>

Day three was one I particularly enjoyed, as it showed how hackers use open-sourced intelligence to their advantage. There is so much publicly available information on the internet, and they showed a few ways to obtain it. In this challenge, I used Google Dorks to find results that aren't usually displayed using regular search terms:

```
inurl:hacking
filetype:pdf “hacking”
site:tryhackme.com
cache:tryhackme.com
```

I also learned about the WHOIS database, which stores public domain information. This can be used to find the domain owner, administrative contacts, and billing or technical contacts. This combined with spear phishing campaigns can be an easy way for a bad actor to access a company's systems. TryHackMe also mentioned the HaveIBeenPwned search, which allows people to search for their email address or phone number in leaked databases.

<br>

<br>

## **Day 4: Scanning Techniques**

<br>

Day four was a repeat of information for me, but good information nonetheless! TryHackMe explained the difference between Passive and Active Scanning. Passive scanning is usually carried out through packet capture like Wireshark, but provides minimal information. Active scanning, which is done by sending packets or queries directly, will typically show more detailed information.

Network Scanning, Port Scanning, and Vulnerability Scanning were all briefly explained. For this challenge, I used Nmap to find a vulnerable Apache server running on a host. I then used Nikto which is an open-source software that scans websites for vulnerabilities.

### **Tools Used**
- **Nmap**: To find a vulnerable Apache server running on a host
- **Nikto**: An open-source software that scans websites for vulnerabilities

I'd prefer using industry standards when it comes to a vulnerability scanner, such as Nessus, but Nikto was easy to use.

<br>

<br>

## **Day 5: Brute Forcing with a Password List**

<br>

Day five reinforced my knowledge of brute force and remote access. TryHackMe covered services like SSH, RDP, and VNC, providing a new insight into Virtual Network Computing (VNC). The challenge involved basic brute forcing using a password list and the Hydra program, utilizing the classic rockyou.txt password list. After successfully finding the correct password, I used the credentials to log into the client via VNC, where I was able to find the flag by viewing the desktop wallpaper.

<br>

<br>

## **Day 6: Email Analysis**

<br>

Day 6 was interesting as it showed how to perform some manual email analysis. Typically a program would do this now, but with phishing still being a massive attack vector in today's world, TryHackMe thought it would be a good idea to show how to analyze emails yourself.

They explained the structure of an email header and what to look for in a quick analysis. This included the following questions:

    Do the "From", "To", and "CC" fields contain valid addresses?
    Are the "From" and "To" fields the same?
    Are the "From" and "Return-Path" fields the same?
    Does the "Message-ID" field exist and is it valid?
    Do the hyperlinks redirect to suspicious/abnormal sites?
    Do the attachments consist of or contain malware?


In the challenge, I used "emlAnalyzer" to view the body of an email and analyze the attachments. The command looked like this:
‘
emlAnalyzer -i Urgent\:.eml --header --html -u --text --extract-all
‘
I also learned to use https://emailrep.io/ in order to investigate the sender of the email. Lastly, I used VirusTotal to analyze the hash of a malicious attachment. TryHackMe also gave a plethora of useful resources when doing malware analysis. Here was the following table provided:
| Tool         | Purpose                                                                                                                        |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| VirusTotal   | A service that provides a cloud-based detection toolset and sandbox environment.                                             |
| InQuest      | A service provides network and file analysis by using threat analytics.                                                        |
| IPinfo.io    | A service that provides detailed information about an IP address by focusing on geolocation data and service provider.          |
| Talos Reputation | An IP reputation check service provided by Cisco Talos.                                                                     |
| Urlscan.io   | A service that analyses websites by simulating regular user behavior.                                                           |
| Browserling  | A browser sandbox used to test suspicious/malicious links.                                                                     |
| Wannabrowser | A browser sandbox used to test suspicious/malicious links.                                                                     |


<br>

<br>

## **Day 7: Malware Analysis Using Cyber Chef**

<br>

Day seven was pretty complex, but through the use of the walkthrough, I was able to get through it. 

It involved some basic malware analysis using a web tool called Cyber Chef to find out what a specific malicious attachment was doing. Using Cyber Chef, https://gchq.github.io/CyberChef/, I constructed the following recipe:

1. Remove obfuscation through regular expressions
2. Locate a string in the code, and single it out by dropping the additional information.
3. Decode the string using base64 to reveal a PowerShell script
4. Decode the PowerShell script using UTF-16LE
5. Find and remove more common patterns in the code to make it more human readable
6. Extract and defang the URLs that the malicious attachment contained

Although this was one of the most difficult days, I really enjoyed it as a way of getting my feet wet in malware analysis.

<br>

<br>

## **Day 8: Blockchain and Smart Contracts**

<br>

Day eight focused on how a decentralized blockchain can be vulnerable to a Re-entrancy attack. This is when a blockchain contract uses a fallback function to continue depleting a contract’s total balance due to flawed logic with the withdrawal function.

Smart contracts are vulnerable to issues such as logic problems and inadequate exception handling. These vulnerabilities are often found in functions, where insecure conditions can be introduced.

As an example, the conditions for the withdraw function are:

    1. Balance is greater than zero
    2. Send Ethereum

At first glance, this may seem secure. However, when is the amount to be sent subtracted from the balance? According to the contract diagram, the balance is only reduced after Ethereum has been sent. This could pose a problem because the function should complete before any other functions can be processed. However, it is possible for a contract to make consecutive calls to a function while the previous call is still executing. In this case, an attacker could continuously call the withdraw function before it can clear the balance, meaning the conditions outlined above would always be met. To prevent this, the function logic must be changed to remove the balance before another call can be made, or stricter requirements must be implemented.

<br>


This challenge required a lot of expertise in understanding blockchain contracts and smart contract functions that are used in today’s cryptocurrency technologies. It shed light on the insecurity of cryptocurrencies due to their decentralized nature.

<br>

<br>


## **Day 9: Metasploit and Meterpreter**

<br>

Day nine was the toughest day of the cyber advent calendar, but I got through it with the help of a helpful walkthrough video. I highly recommend giving this challenge a try if you want to learn about real-world exploitation.

It began with a nmap scan of a box that revealed a vulnerable Laravel web application with the CVE-2021-3129 vulnerability. I set up a pivoting intermediate server using Socks Proxy and then selected a common exploit using Metasploit. I upgraded the session to a Meterpreter session with the "sessions -u -1" command.

After some difficulty accessing the Meterpreter session, I was able to find a .env file with credentials for the web application. Using the found credentials, I was able to access the flag after a successful SSH session with the root credentials.

I loved the challenge and its difficulty, and I look forward to learning more about Metasploit and Meterpreter as they are incredibly powerful tools for penetration testers.

<br>

<br>

## **Day 10: Video Game Hacking Using Cetus**

<br>

Day ten was a fun challenge that focused on modifying memory in programs. It involved altering the memory of a Web Assembly-based game created for the event. After installing Cetus on Firefox, I was able to locate hexadecimal values that changed when my in-game health decreased. By converting the hexadecimal values to their string counterparts, I was able to manipulate them.

I successfully solved an unsolvable riddle, passed an impossible challenge, and discovered the challenge flag all through Cetus' memory manipulation.

Although this challenge was enjoyable, it only touched upon the topic of memory manipulation. Most modern video games are secure from memory manipulation as they are not developed using Web Assembly, but the concepts learned will be useful for future projects.

<br>

<br>

## **Day 11: Memory Dump Forensics**

<br>

Day eleven involved analyzing the memory dump of a hacked machine. I used a program named Volatility to determine the OS of the dump, examine running processes, and investigate connections at the time of capture. Volatility is an excellent tool for analyzing memory dumps, allowing one to list processes, network connections, and view the contents of the clipboard, notepad, or command prompt. After finding a file that was uploaded by a suspicious user, the challenge was completed.

I look forward to analyzing more memory dumps in the future as I found it very entertaining. 

<br> 

<br>

## **Day 12: Malware Forensics**

<br>

For day twelve, the task was to analyze a sample of malware. This challenge was a good opportunity for a beginner in malware analysis to hone their skills. It was emphasized that analyzing malware must be done in a sandbox for safety reasons. The challenge covered both static and dynamic analysis. In static analysis, the code is analyzed without execution, while in dynamic analysis, the code is executed in a secure environment.

For this challenge, I used a program called Detect It Easy (DIE), which provides information on files by looking at its code (static analysis). I also used Process Monitor from Windows to show what changes the malware made to the file system (dynamic analysis). This malware made changes to the registry, file system, and network as shown by Process Monitor. 

<br>

<br> 

## **Day 13: Wireshark**

<br>

Day thirteen was about analyzing a packet capture (PCAP) using Wireshark. Though automated systems monitor PCAPs, it's still important to understand manual packet analysis. The challenge involved analyzing a PCAP of a network that was hacked, with the goal of figuring out how it was compromised. 

First, I looked for DNS packets to find sites with active conversations with the machine. Using Wireshark, I saw a malicious file was downloaded from one of the domains. I hashed the file and used VirusTotal to determine it was a known piece of malware. 

I enjoyed this challenge as Wireshark is an extremely valuable tool in network analysis and I look forward to using it in a similar manner in the future.

<br>

<br>

## **Day 14: The IDOR Vulnerability**

<br>

Day fourteen started off explaining the importance of The Open Web Application Security Project (OWASP) Top 10. This list is very important when it comes to securing your web application as it includes the 10 most common and “trendy” ways for a web application to be hacked. You can bet that every highly rated web application out there is secured from at least these 10 as well as any that have been on previous year’s OWASP Top 10. 

In this challenge, I performed a very simple hack that involved manipulating the values of the data base request just by changing numbers in the URL. This is called IDOR. “IDOR refers to the situation where a user can manipulate the input to bypass authorization due to poor access control. IDOR was the fourth on the OWASP Top 10 list in 2013 before it was published under Broken Access Control in 2017.”

After manipulating the last value of the following URL:


http://santagift.shop/account/user_id=132


I was able to find multiple different users. After enumerating the server by checking each available user ID, I was able to find the flag. Although it was simple, its a very effective form of web server request manipulation. I enjoyed this one very much just due to the simplicity of it. I’m sure I’d never find this vulnerability out in the wild, or at least I hope!

<br>

<br>

## **Day 15: File Input Validation**

<br>

Day fifteen was one of my personal favorites as it focused on Unrestricted File Upload vulnerabilities and taught me how to prevent them with some input validation.

In this task, I used a C# file upload as the case study. C# is a popular language used to create both Windows application and web applications at large organisations.

 I looked at a simple website that had the ability for the user to upload a resume to the website. However, it wasn’t just allowing resumes, it was allowing all file types to be uploaded! This vulnerability can lead to some series cross-site scripting (XSS) or cross-site request forgery (CSRF). A bad adversary can ultimately use this to take control of the entire web server! This is why input validation is extremely important for file upload web services.

<br>

**Validating Input**

To validate the user input for the challenge, I needed to make sure that the file uploaded was a PDF as that is the only allowed file type. 

```
string contentExtension = Path.GetExtension(fileUpload);
if !(contentExtension.equals("PDF"))
    {
        allowed = False;
     }
```

Then, once the extension of the file is verified as a PDF, the file size should also be validated. This is because the webservice could be intentionally slowed or even disrupted if someone were to try and upload PDFs that were far too large for the webserver.
         
```
int contentSize = fileUpload.ContentLength;
//10Mb max file size
int maxFileSize = 10 * 1024 * 1024
if (contentSize > maxFileSize)
    {
        allowed = False;
    }
```

Once the size is verified to be under the maximum limit, the file needs to be renamed. Even though the uploads are stored outside the web root, an attacker could use a file inclusion vulnerability to execute a file. Therefore the uploaded file must be randomly named to prevent a bad advisory from being able to reference or recover their file.
   
```
Guid id = Guid.NewGuid();
var filePath = Path.Combine(fullPath, id + ".pdf");
```
Lastly, the file should still be scanned for malware. Even though the file can be a PDF under the specified file size, it can still contain malicious content for PDF readers. This is why the files must always be scanned for malware as well.
```
var clam = new ClamClient(this._configuration["ClamAVServer:URL"],Convert.ToInt32(this._configuration["ClamAVServer:Port"])); 
var scanResult = await clam.SendAndScanFileAsync(fileBytes);  
  
if (scanResult.Result == ClamScanResults.VirusDetected)
    {
        allowed = False;
    };
```
This concluded the entire input validation for a field in which the web server stores user’s resumes. I really enjoyed this one as I've recently been uploading my resume to possible employer’s sites and each time it reminds me of this vulnerability. Even though input validation is becoming extremely common, it is still possible to find websites that have vulnerable input fields. I really enjoyed this one because instead of only performing the hack like I have many times before, it showed how to prevent it. This is ultimately what I will be doing in the field as a penetration tester so this challenge was perfect.

<br>

<br>

## **Day 16: SQLi Input Validation**

<br>

Day sixteen was another one of my favorites from the Advent of Cyber. Like the previous day, I learned not only how a more complex SQLi works, but also how to validate the input of SQL commands to prevent an SQLi vulnerability.

In the challenge, I learned that it’s best to change the any input into the desired data type needed right away. For example, I changed the first input to an integer using the intval() function as a number is needed for the input. This will prevent an injection of SQL code from running as well since the intval() function will immediately convert any string to 0. 
I changed:

```
$query="select * from users where id=".$_GET['id'];
```

To the following:

```
$query="select * from users where id=".intval($_GET['id']);
```

Forcing the input of the SQLi into a string will prevent it from running, but it it is recommended to use prepared statements for SQL input fields. This involves having a predetermined format for expected responses, thereby avoiding any exploitation of the web server by SQLi.

For example, this code was first expected to take a single SQL query string which would be vulnerable to SQLi. 

```
$q = "%".$_GET['q']."%";
mysqli_stmt_bind_param($stmt, 'ss', $q, $q);
```

It was then turned into the following prepared statement, shown by $query.

```
$q = "%".$_GET['q']."%";
$query="select * from toys where name like ? or description like ?";
$stmt = mysqli_prepare($db, $query);
mysqli_stmt_bind_param($stmt, 'ss', $q, $q);
mysqli_stmt_execute($stmt);
$toys_rs=mysqli_stmt_get_result($stmt);
```

The latter code turns the initial SQL query into a statement that requires dynamic parameters. This allows the database to stick the pieces together securely without depending on anything else. With dynamic parameters and a set statement, there won’t be any SQLi exploitation.

This challenge was fun but difficult due to my novice PHP coding skills. However, I have performed many SQL injections before using BurpSuite on https://portswigger.net, so the concept wasn’t foreign to me. I enjoyed learning how to prevent an SQLi instead of only performing one again.

<br>

<br>

## **Day 17: Regex, More Than Just Input Validation**

<br>

Day seventeen was a perfect addition to the previous days regarding input validation. Although input validation is required for all web apps, it isn’t the only thing that should prevent input vulnerabilities. No web application can be completely XSS-proof through input validation alone. Layers of security are important when securing anything. It’s important to never have only one point of failure. 

There are various tools and frameworks that help secure applications, such as HTML5. However, this challenge used regex specifically due to its capability to implement filters. While input validation focuses on server-side validation, a filter is a client-side tool that will help prevent attacks such as XXS or SQLi. 

To complete the challenge, I needed to create the following filters in regex:
1. Filtering for Usernames: 
    - Alphanumeric
    - Minimum of 6 characters
    - Maximum of 12 characters
    - May consist of upper and lower case letters
2. Filtering for Emails: 
    - Follows the form "local-part@domain" (without quotation marks)
    - Local-part is a random string
    - Domain is in the form of "<domain name>.tld"
    - All top-level domains (tld) are ".com"
3. Filtering for URLs: 
    - Starts with either http or https
    - Some of the URLs have "www"
    - A TLD should exist
After learning a little bit more about regex filters, I was able to find the flags and answers to the challenge using the following filtered commands.

**Answers:**

Filtering for Usernames:

```
Egrep ‘^[a-zA-Z0-9](6,12)$’ strings
```


Filtering for Emails:

```
Egrep ‘^.+@.+\.com$’ strings
```


Filtering for URLs:

```
Egrep ‘^http(s)?.{3}(www)?.+\..+$’ strings
```

This challenge was a good reminder that you need more than just a single point of failure for anything to be secure. The filters I created in order to egrep specific details out of a file can be used as client-side validation in web applications. This will prevent any clear text from running that can result in attacks like XSS or SQLi. 

<br>

<br>

## **Day 18: Log Threat Detection using Sigma Rules**

<br>

Day eighteen was an extremely helpful day as it helped me understand more about the power of threat detection through logs. I know there are many programs out there that can automate detection very easily, but it was nice learning how to create detection rules myself using Sigma. 

This challenge involved me creating Sigma rules that would allow for the detection of malicious activity. Starting with the basics, I would first create a rule that detects suspicious local account creation. 

Each rule needs the following variables filled out in order to work.

**Sigma Rules**

- Title: Names the rule based on what it is supposed to detect.
- ID: A globally unique identifier in UUID format, used to maintain order of rules submitted to the public repository.
- Status: One of five declared statuses - Stable, Test, Experimental, Deprecated, or Unsupported - that describe the maturity of the rule.
- Description: Detailed context about the rule and its intended purpose.
- Logsource: Specifies the log data for the detection, including optional attributes: Product, Category, Service, Definition.
- Detection: Required field that outlines the parameters for malicious activity to trigger an alert, with search identifiers and a condition expression.
- False Positives: List of known false positives that may occur based on log data.
- Level: Severity level of the activity under the written rule, with five levels: Informational, Low, Medium, High, Critical.
- Tags: Information to categorize the rule, often associated with tactics and techniques from the MITRE ATT&CK framework, with predefined tags defined by Sigma developers.

Here is an example of a Sigma Rule after it has been created.

```
title: Suspicious Local Account Creation
id: 0f06a3a5-6a09-413f-8743-e6cf35561297 
status: experimental
description: Detects the creation of a local user account on a computer.
logsource:
  product: windows
  service: security
detection:
  selection:
    EventID:  # This shows the search identifier value
      - 4720    # This shows the search's list value
  condition: selection
detection:
  selection:
    Image|endswith:
      - '\svchost.exe'
    CommandLine|contains|all: 
      - bash.exe
      - '-c '   
  condition: selection
falsepositives: 
    - unknown
level: low
tags:
   - attack.persistence # Points to the MITRE Tactic
   - attack.T1136.001 # Points to the MITRE Technique
   ```

After creating a few of these rules for user account creation, file upload, and executables ran, I was able to find the flags and solve the challenge. This challenge was extremely helpful in understanding what an experienced SOC Engineer may do in order to detect malicious activity on a server.

<br>

<br>

## **Day 19: Hardware Hacking using Saleae**

<br>

Day nineteen was easily one of the most difficult days during the Advent of Cyber. It focused on hardware hacking through analyzing the logic data dump of specific hardware. This was done using a logic analyzer called Saleae. Although there was an excellent walk through, it took me a few reads in order to fully understand what I was doing. 

Hardware hacking relies completely on altering the physical communication between two pieces of hardware. This is done by analyzing and altering the bits of the electrical communication, or by changing the 1s and 0s. Although I don’t ever find myself hardware hacking in the future, it was still interesting to learn the basics of how it is performed. This hardware hacking can be useful in understanding who is a responsible for rogue implants on hardware or figuring out what is being transmitted.

The challenge was a difficult version of trial and error. I changed many variables until eventually finding human readable information from 1s and 0s. This involved calibration, adding digital channels, identifying the protocol, and adding multiple serial analyzers to turn that information into useful data. This was all done through Saleae which seemed like a very helpful program when performing hardware hacking or analysis. 

<br>

<br>

## **Day 20: Firmware Reverse Engineering**

<br>

Day twenty was about reverse engineering firmware. Firmware provides low-level control for the designer and developer to make changes at the root level and allows hardware to communicate with other software running on a device. Reverse engineering involves working backwards through code to figure out its functionality, usually for security purposes. There are two techniques: static and dynamic analysis.

**Static Analysis**

This involves examination of the binary file contents and reading assembly instructions to understand the functionality of the firmware. Here are some tools that are used for static analysis.
* **BinWalk**: This extracts code snippets inside any binary by searching for file formats like zip, tar, ELF and exe. 
* **Firmware ModKit (FMK)**: This is widely used to extract firmware using binwalk and outputs a directory with the firmware file system. The code can be modded and repacked into the binary using this tool.
* **FirmWalker**: This tool searches through extracted firmware file system for unique strings and directories. Here are some of the strings that are searched: etc/shadow, etc/passwd, etc/ssl, admin, root, password, ssh, telnet, netcat, and more.

**Dynamic Analysis**

This involves running the firmware code on actual hardware and observing its behavior through emulation and hardware/software based debugging. One of the significant advantages of dynamic analysis is to analyze unintended network communication. Here are some tools that are commonly used for dynamic analysis.

* **Qemu**: This is a free and open-sourced emulator that works on cross-platform environments. It provides ways to emulate binary firmware for different architectures like Advanced RISC Machines (ARM), Microprocessors without interlocked pipelined stages (MIPS), and more on the host system. 
* **Gnu DeBugger (DGB)**: A dynamic debugging tool for emulating a binary and inspecting its memory and registers. It supports remote debugging which is very useful when running on a versing the firmware from a separate host.

The challenge involves a step-by-step guide to a basic reverse engineering process. It involves attempting to decrypt the firmware using older versions to extract a public and private key with FMK. This allowed the decryption of a gpg encrypted file, which led to the flags.

<br>

<br>

## **Day 21: Webcam Hacking**

<br>

Day twenty-one starts off by explaining IoT devices and the increasing use of these devices in everyday lives. Below is brief synopsis of the popular messaging protocols used by IoT devices.

| Protocol | Communication Method | Description |
| --- | --- | --- |
| MQTT (Message Queuing Telemetry Transport) | Middleware | A lightweight protocol that relies on a publish/subscribe model to send or receive messages. |
| CoAP (Constrained Application Protocol) | Middleware | Translates HTTP communication to a usable communication medium for lightweight devices. |
| AMQP (Advanced Message Queuing Protocol) | Middleware | Acts as a transactional protocol to receive, queue, and store messages/payloads between devices. |
| DDS (Data Distribution Service) | Middleware | A scalable protocol that relies on a publish/subscribe model to send or receive messages. |
| HTTP (Hypertext Transfer Protocol) | Device-to-Device | Used as a communication method from traditional devices to lightweight devices or for large data communication. |
| WebSocket | Device-to-Device | Relies on a client-server model to send data over a TCP connection. |

<br>

Attackers can discover device behavior from communication sniffing, source code analysis, or documentation.

* **Communication sniffing** involves monitoring the communication between devices to determine the protocol used, the address of the middleware or broker, and the behavior of the communication. For example, if unencrypted HTTP requests are sent to a central server and then translated into CoAP packets, we can observe the HTTP packets and search for topics or message formats that vendors may try to hide, such as settings, commands, etc. to interact with the device.

* **Source code analysis** involves examining the source code of a device to gain insight into how it processes and uses data. This method of identification can provide similar information to communication sniffing but is generally considered more reliable and provides more definite information.

* **Documentation** provides a comprehensive understanding of the standard functions of a device or endpoint. However, relying solely on documentation as a means of identification may not include sensitive payloads, topics, or other information that attackers need but is not normally relevant to end users.
 
The challenge then walks through how to exploit a webcam using the mosquito_sub client to subscribe to a MQTT broker.

* Verify that MACHINE_IP is an MQTT endpoint and uses the expected port with Nmap.
* Use mosquitto_sub to subscribe to the device/init topic to enumerate the device and obtain the device ID.
* Start an RTSP server using rtsp-simple-server
```
docker run --rm -it --network=host aler9/rtsp-simple-server
```
Note the port number for RTSP; we will use this in the URL you send in your payload.

If you are having issues receiving a connection and are confident that your formatting is correct, you can attempt to use a TCP listener:

```
sudo docker run --rm -it -e RTSP_PROTOCOLS=tcp -p 8554:8554 -p 1935:1935 -p 8888:8888 aler9/rtsp-simple-server
```
* Use mosquitto_pub to publish your payload to the device/<id>/cmd topic.
        Recall that your URL must use the attackbox IP address or respective interface address if you are using the VPN and be in the format of rtsp://xxx.xxx.xxx.xxx:8554/path
        If the message was correctly interpreted and the RTSP stream was redirected the server should show a successful connection and may output warnings from dropped packets.
* You can view what is being sent to the server by running VLC and opening the server path of the locally hosted RTSP server.
```
vlc rtsp://127.0.0.1:8554/path
```
If you are using Kali, you must download VLC from the snap package manager to ensure the proper codecs are installed.

The idea of IoT hacking is still very new to me but with challenges like this, it slowly becomes easier to understand. I look forward to the day when I'm able to hack and secure more IoT devices as it's an growing attack vector that can lead to some serious damage.

<br>

<br>

## **Day 22: Attack Surface Reduction**

<br>

Day twenty-two was wrapping together some of the concepts learned in the previous challenges to reduce the attack surface a bad actor would have. Here are some steps that can be taken to minimize an attack surface.

* **Close the ranks:** Close any open ports that should not be accessible, such as the open SSH port mentioned in the challenge.

* **Put up the shields:** Although the SSH port is protected by password, it wasn’t strong enough to resist a brute-forcing attempt. Ensure passwords are strong enough to resist brute-force attacks. A stronger password policy and a timeout after five incorrect attempts can increase the time required for brute-forcing.

* **Control the flow of information:** Make sure sensitive information, such as credentials, is not committed to public repositories, like GitHub.

* **Beware of deception:** Enable protection against phishing emails by filtering out spoofed and malicious emails on the email server or using email aliases for personal accounts.

* **Prepare for countering human error:** Disable macros on end-user machines to prevent the risk of malicious macro-based documents used in phishing emails.

* **Strengthen every soldier:** Implement attack surface reduction rules on every machine to protect sensitive information. Microsoft’s Attack Surface Reduction Rules can be a good starting point.

https://learn.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules-reference?view=o365-worldwide

* **Make the defense invulnerable:** Use vulnerability scans to identify and patch any vulnerabilities in internet-facing infrastructure.

Overall, these steps serve as a reminder of how knowledge of hacking can be used to prevent potential threats. The challenge was a fun way to apply these concepts which resulting in finding the flag.

<br>

<br>

## **Day 23: Defensive Mindset**

<br>

Day twenty-three was the last day of the Advent of Cyber. It focused on reinforcing the defensive mindset necessary for protecting against cyber attacks. The concepts discussed included:

    Layering defenses
    Reducing attack surfaces
    Writing secure code
    Analyzing different parts of the attack chain
    Practicing defense in depth

“The core mindset that Defense in Depth is founded on is the idea that there is no such thing as a silver bullet that would defeat all of an organization’s security woes. No single defense mechanism can protect you from the bad world out there.”

The challenge was a pre-built game that helped demonstrate the importance of having a well-rounded defense strategy at different levels, including:

1.	The first level is having a focus on perimeter security. There are great prevention mechanisms present in the perimeter and essentially complete trust within it; thus, once the perimeter is bypassed, the organization is pretty much at the mercy of the adversary.
2.	The second level has defensive layers in place; however, the emphasis is solely on prevention. It doesn’t leverage ‘knowing your environment’; even though adversarial objectives may be prevented to some degree, there’s a missed opportunity in terms of detection and consequently, alerting and response. Prevention is good, but the key to defeating the bad guys is having visibility into what they are doing.
3.	The third level has well-rounded defensive layers in place, leveraging the strategic application of sensors, effective creation of analytics, and efficient alerting and response capabilities of the security team. Preventative measures here are not only coupled by detection and alerting but also by immediate and efficient response.

By following the steps in the game, the flag was obtained and the challenge was completed.

<br>

<br>

<br>

## **Concluding Thoughts**

<br>

The Advent of Cyber 2022 provided a comprehensive and immersive learning experience. It covered a range of topics, from the basics of hacking to the latest security techniques and technologies. The challenges helped reinforce my understanding of the various concepts and provided practical insights into how they can be applied in real-world scenarios. Additionally, the emphasis on defense in depth and the need to continuously evolve and adapt to new threats reinforced the importance of staying vigilant and proactive in the field of cybersecurity. Overall, the Advent of Cyber 2022 was a valuable experience that has helped me better understand the intricacies of cybersecurity and how to protect against cyber threats. 
