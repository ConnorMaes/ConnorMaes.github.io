# **Phishing Email Analysis**

<br>

<br>

### In today's world, email analysis is an essential skill to possess. Phishing and social engineering continue to be the most successful attack methods employed by malicious actors to infiltrate your systems. With the emergence of open-source AI technologies such as ChatGPT, phishing emails are becoming more prevalent and harder for end-users to identify. Here's a brief guide on how to quickly analyze an email.

<br>

<br>

<br>

## **Analyze the Email Header**

<br>

Email header analysis is the most effective way to determine if an email is malicious, regardless of the content. Here are some questions you can ask yourself when looking at an email header.

* **Are the "From" and "Return-Path" fields the same?**
* Do the "From", "To", and "CC" fields contain valid addresses?
* Are the "From" and "To" fields the same?
* Does the "Message-ID" field exist and is it valid?
* Do the hyperlinks redirect to suspicious/abnormal sites?
* Do the attachments consist of or contain malware?

<br>

<br>


Now, let's begin on how to quickly analyze an email header. This process will assume you're using gmail, although it can be done in any email service.

1. In the email, select the three vertical dots and click "Show Original".
    * This will show you the email header and the contents of the email in text.
2. Copy the contents of the Header.
    * The Header will typically end with the "List-Id". 
3. Go to MxToolBox to analyze the Header.
    * https://mxtoolbox.com/Public/Tools/EmailHeaders.aspx
    *  Paste the header in the "Paste Header:" field and click "Analyze Header".
4. Look at the "Delivery Information" section for a quick run-down.
    * Any of the checks that have a red circle and X means the email didn't pass that test.
    * To see what these tests are checking for, you can scroll down.
5. Check the reputation of sender at Emailrep.io
    * Go to https://emailrep.io/
    * In the Email Header, the sender's email will be after "From:"
    * Make sure to only copy/paste the email itself. Any prior names will result in an error on emailrep.io.

<br>

<br>

<br>


## **Analyze the Contents of the Email**

<br>

Analyzing the header will typically be enough to tell if an email is malicious. However, in the situation that an end-user comes to you and says they clicked on a link or downloaded a document and opened it, it may be in your best interest to analyze the contents of a malicious email. 

An easy tool that is used to analyze both files and URLs from emails is Hybrid Analysis. 

https://www.hybrid-analysis.com/

This website is a free malware analysis service that detects and analyzes unknown threats using a unique hybrid analysis technology. 

<br>

It's very important to only download attachments and handle suspicious links with the use of a sandbox. A home lab sandbox can be as simple as hosting a VM on your computer that isn't connected to any personal information including your main email or social media accounts. However, it is typically recommended to use a highly rated sandbox software such as Cuckoo or Windows Sandbox. The idea is to separate as much of your information as possible from the sandbox's operating system. This includes segmenting the sandbox on a VLAN that can't see any other devices, not using it to log into any personal accounts, and never using it to browse the internet. This will allow any accidents in malware analysis to be slightly more forgiving. There are rare situations in which malware can escape a VM and infect the host computer, so it is still best to handle malicious links and files with extreme care.

<br>

Here's a tutorial on analyzing the malicious URL or file quickly.

1. Forward the phishing email to an email alias that you have access to.
    * This MUST be done prior to using your sandbox. You must not login to any of your personal accounts on your sandbox.
    * This email alias should not be used for anything other than email analysis. If this is connected to you in any way, it can be just as bad as opening the malware on your host machine.
2. On your sandbox, log into the email alias that you forwarded the phishing email to.
    * If the malware is accidentally opened and happens to take over your email, it isn't connected to you in anyway, and that email can simply be deleted.
3. Download any attachments or copy any URLs that were included in the phishing email. 
    * Although you are now in a sandbox, still be VERY CAREFUL not to click the link or open the attachment once downloaded.
    * There are certain types of malwares that specializing in escaping VMs and infecting host machines, so it’s best to proceed with extreme caution.
4. Paste the file or URL into the File/URL field at https://www.hybrid-analysis.com/
    * You are given the option to include your email to be notified when it is completed, as it can take 5-20 minutes.
5. Consent to the terms and data protection policy, prove you're not a robot, and click continue.
6. You can now select the operating systems in which you wish to test this malicious file or URL and click "Generate Public Report".
    * This website will use analysis from websites like VirusTotal, URLscan.io, and more.
    * It will hash the file or URL and analyze it as such.
    * It will also open the file in a Falcon Sandbox and include the results.
    * There is also information on Incident Response, MITRE ATTACK Techniques Detection, and Additional Context.


<br>

<br>

Here are some additional tools for email analysis:

| Tools | Purpose |
| --- | --- |
| VirusTotal   | A service that provides a cloud-based detection toolset and sandbox environment.|
| InQuest      | A service provides network and file analysis by using threat analytics.|
| IPinfo.io    | A service that provides detailed information about an IP address by focusing on geolocation data and service provider.|
| Talos Reputation | An IP reputation check service provided by Cisco Talos.|
| Urlscan.io   | A service that analyses websites by simulating regular user behavior.|
| Browserling  | A browser sandbox used to test suspicious/malicious links.|
| Wannabrowser | A browser sandbox used to test suspicious/malicious links.|


<br>

<br>

I hope you enjoyed this quick tutorial on how to quickly analyze a phishing email. For serious threats, the malware analysis portion will need to be done by professionals, but you can still follow some universal best practices:
* Turn off a computer if it may have been infected.
* Notify your team about the possibly infected machine.
* Segment that computer from the network.
* Reset that computer's and user's passwords immediately.

<br>

Thanks for reading and have a great day!

