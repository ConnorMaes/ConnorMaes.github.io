# **Flipper Zero Penetration Testing**
<br>

## What I learned in a month of penetration testing with my Flipper Zero

<br>

<br>

Having a Flipper Zero has taught me what 'hidden' technologies I interact with on a daily basis. I've learned so much about physical penetration testing through the use of Flipper Zero; NFC hacking, WiFi hacking, RFID hacking, and more! I figured I'd share a short post about everything I've learned so far from this neat educational tool.

<br>

<br>

### **Infrared**

<br>

The very first hack I used with my Flipper Zero was a universal remote. With my roommates’ large entertainment system, this was very fun. As soon as I unboxed the Flipper Zero, I immediately set up a universal remote to be able to operate a Vizio TV, a LG TV, and a Sony Sound System. Seeing my roommates' faces when the volume went from 20 to 100 with no one touching the remote was priceless. 

<br>

<br>

### **Bad USB**

<br>

Although that universal remote has been the most useful feature of my Flipper Zero, I quickly started working with the Bad USB features of the Flipper. I started simple with some ducky scripts that can be found on GitHub and tailored them to my liking. It started off with the 'prank' ducky scripts. This included Rick Roll after Rick Roll. I quickly ventured into the destructive side with some scripts like disabling windows defender, adding a local admin, and enabling RDP. I started with individual scripts for each of these, but eventually combined multiple ducky scripts to one. I used my computer to continuously check if the ducky scripts would work well on a Windows 10 Pro version. However, you can only get so far with using ducky scripts to do stuff on certain operating systems. Every computer loads at different speeds, each version of windows may have a slightly different user interface, and every computer may have varying security. This is when I realized the best ducky script is one that goes to a website that would then run a script tailored to your needs. Although I've yet to make my own website for penetration testing purposes, I would like to do this in the future and use it alongside my Flipper Zero Bad USB.

<br>

<br>

### **NFC**

<br>

After testing out nearly every ducky script, I thought it was time to move onto NFC testing. This is primarily used to steal card information. I entertained the idea of stealing credit card information from myself or from my friends (for educational purposes, with their permission!). I realized that my wallet was different than most of my friend's wallets in that it did not block NFC frequencies. Most new wallets prevent this nowadays, but there are still some that do not block these frequencies. I demonstrated to many of my friends how easy it is for a bad actor to steal their credit card information. Although it would be hard for them to use, a bad actor could write that information to a fake credit card and use it shortly after stealing it to remain under cover from the bank.

<br>

<br>

### **Sub-GHz**

<br>

NFC was entertaining when it came to a cool party trick, but I still wanted more from my Flipper. I wanted to open Tesla charge ports, brute force my garage code, and hack into some IoT devices. The Tesla charge ports script was easy to obtain but difficult to use with people permission. It's an easy sub-GHz transmission, that you can find on google by just searching "Flipper Zero Tesla". However, I eventually found a willing Tesla owner who let me try my shenanigans. They were starstruck! They said, "That's what happened to me before!". I felt like I was already a penetration tester letting a client know how they were once hacked. This was peak Flipper Zero entertainment. 

Next, I downloaded a sub-GHz brute forcing tool that could be used to open garage doors, gated communities, specific building doors, and more. I quickly realized that the sub-GHz used to do this was not allowed on the base Flipper Zero firmware, as it is illegal to transmit at that frequency in the US. I also tried to brute force the smart lightbulbs that can be manipulated using sub-GHz frequencies, but it seems that those frequencies were also permitted to transmit in the U.S. If I would like to do this, I'd need to download and install the unleashed firmware. Unfortunately, I won't be doing that as the transmission of those frequencies is permitted in the U.S.

I'm a little nervous to try messing around with capturing specific car keys. I don't have any keys to try that with of my own since I have an elite, un-hackable car that's manual in every way. I'd need to try this out on a friend's car. It can be easy to lock a set of keys out due to a car key's rolling number authentication. A rolling number means that every time the car is unlocked, it transmits a number +1 from the previous unlock signal. This is used to prevent hackers from gaining access to a car just from repeating the signal. However, this rolling code can sometimes have vulnerabilities. If a specific rolling number is used twice, it will prevent all signals from unlocking the car as a defense mechanism. Depending on the car, the defense mechanism may prevent anyone from unlocking the car which would mean locking the owner out of their car. Sometimes other rolling codes from similar branded keys can unlock the car as well even if they weren't a continuation of the previous unlock signal. This is because the car doesn't have a way to verify backup rolling codes as if they were a backup key. I'll keep this idea in my back pocket as I'm not trying to lock anyone out of their cars or pay for a pricey key replacement. 

<br>

<br>

### **WiFi**

<br>

I wanted to start diving into the WiFi dev board that I bought alongside the Flipper Zero to see if I could tryout some WiFi hacking as well. First, I installed Marauder firmware onto the WiFi dev board. I heard Marauder was a good tool to use alongside an Android phone command line app. After some complications and troubleshooting, I was able to use my Android connected to my Flipper Zero with the WiFi Dev board attached to do some WiFi pen testing. I learned that WiFi hacking is very much do or don't. Depending on the security of a WiFi network, it is very difficult to do any attacks at all. Typically, WPA2 and WPA3 are secure enough that none of the attacks would work. However, I could use it to scan for networks that are unsecure. I could also use it to perform an AP clone spam, where many wireless access points would be produced to confuse those trying to connect to a real WiFi network. "Which WiFi network is the real one?" Although, I wasn't very impressed with that. The juicy hack was doing a deauthentication attack, where everyone would be disconnected from the specified WiFi network. However, when first attempting on my local WiFi, I was not able to deauth anything. After some more research, I learned that WPA2 and WPA3 networks with updated hardware have 802.11w capabilities, which protect against deauth attacks. After some bar hopping in my local city, I was eventually able to find some unsecured WiFi networks and successfully deauth them using the Flipper. Just like that, I was already performing real WiFi pen testing that a company would charge for, albeit an easy check.

<br>

<br>

### **Closing Thoughts**

<br>

Being a penetration tester has always been a dream job and I've never felt more like one before with the Flipper Zero. I'm excited to continue learning more about physical penetration testing techniques as I hope to someday be an accomplished white hat hacker. If you've made it this far, I appreciate the reading and I look forward to providing you updates on my Cyber Security journey. 

