# **Setting Up Active Directory**

<br>

<br>

### Structure is essential for securing data. Active Directory provides just that, offering a centralized management system for the majority of businesses in the U.S. By using systems like Active Directory, companies can steer clear of the headaches caused by disorganization, shadow IT, and security threats that can compromise their computer systems.

<br>

In this tutorial, I'll be using Active Directory to get familiar with PowerShell scripting. Eventually, I'd like to create a *single* PowerShell script that creates a practical AD forest to use for penetration testing. 

<br>

<br>

## **Quick Overview**

<br>

Setting up Active Directory (AD) involves installing the necessary software on a server, configuring the AD forest and domain structure, and creating and managing user and computer accounts. Here is a high-level overview of the process:

    1. Install the Active Directory Domain Services (AD DS) role on a server running Windows Server operating system.

    2. Promote the server to a domain controller by running the Active Directory Domain Services Configuration Wizard. This will create the initial AD forest and domain.

    3. Configure the AD forest and domain structure.

    4. Create AD users.

    5. Domain join your computers. 

    5. Assign permissions and access control to AD resources through the use of GPOs.

Please note that this is a basic overview and further steps and considerations may apply depending on the size and complexity of an organization's environment.
For my Windows Server VM, I'll be using my home lab's server which is running Proxmox (https://www.proxmox.com/en/) to handle the VMs. For easy access to the server, I'll be using RDP with local authentication for GUI access.

<br>

<br>

<br>

## **1. Install Active Directory Domain Services on a Windows Server VM.**

<br>

You'll need to start by setting up your Windows Server Machine, whether it's an actual server in your house or VM is up to you. An ISO for any version of Windows Server can be obtained from the following microsoft link: 

https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019

Once your Windows Server OS has been installed and booted, you can then follow the steps below to install Active Directory Domain Services (ADDS).

* Start by opening Server Manager although most versions of Windows Server will automatically start this when booted up. 
* Click on "Add Roles and Features".
* A wizard will start that will guide you through the process. You can click on "Next" for the "Before You Begin" page.
* Next, choose your Installation Type. This can be Role-based or feature-based installation. For my case, I will be using a Role-based or feature-based installation. Click "Next".
* Select the destination server. In my case, it showed up as YOSHI SERVER as that's what I renamed the windows server box to. Click "Next".
* Then you will select the Server Roles. This is where you will see a list of many roles. For my case, I will be selecting the following as this will be used for various projects.
    * Active Directory Domain Services
    * DHCP Server 
    * DNS Server 
    * File and Storage Services > Storage Services
    * File and Storage Services > File and iSCSI Services > File Server
DHCP and DNS aren't typically installed on a company's domain controller, but since I will be using this for various projects, I decided to select them.
* Once those roles have been selected, click "Next".
* On the Select Features page, click "Next" to lead you to the AD DS page.
* Click "Next" again on the AD DS page.
* Finally, click "Install" to begin installing your selected roles.
* Once it is done installing, click "Close".

Luckily the installation wizard makes the first step very simple. Another way is to use the following PowerShell command obtained from learn.microsoft.com:
``` PowerShell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```

<br>

<br>

## **2. Create your root domain and promote Windows Server to a Domain Controller.**

<br>

After installing AD DS and any other roles desired, the Windows Server will require a reboot. Once rebooted the root domain can be created and the server can be promoted to a Domain Controller. For my root domain, I'll be using yoshi.local. "Yoshi" because that's my dogs name and "local" because it's only going to be used for home lab purposes. You can make your root domain anything you'd like if you’re using it for home lab purposes, but I'd stick to something that looks like a fully qualified domain name (FQDN) for simplicity.

* To do this, click "Manage" in the top right corner of Server Manager.
* Click "Promote this server to a domain controller". This will  open the AD DS Configuration Wizard. 
* Here, I will be selecting "add a new forest", and I will be using yoshi.lol as the root domain name. Click "Next".
* On the Domain Controller Options page, leave the default configuration and input your password. Then select "Next".
* On the next page, DNS Options, you will likely see an error. Ignore it and click “Next“.
* Then choose your NetBIOS Domain Name, or leave it as what is already listed there. In my case it was "yoshi" as the root domain is yoshi.lol. Click "Next".
* Leave the paths as default and click "Next".
* Review your options on the next page and click "Next" if everything looks correct.
* In the next step, prerequisites will need to be validated before AD DS is installed. If there are any errors in your previous steps, this page will show you where and you'll need to fix it. Click "Install".
* Once it is completed installing, the server will reboot and you can log into the domain with the credentials that you set up in the previous steps.

I prefer to do this through the Server Manager as I've done it before, but there are PowerShell scripts that can promote Windows Server to a Domain Controller.
``` PowerShell
$Domain = Read-Host "Enter the domain name in which the forest will be created (e.g. testdomain.local or google.com)"
$NetbiosName = Read-Host "Enter the net bios name for your domain (e.g. 'TestDomain' or 'Yoshi')"
$safeModeAdminPassword = Read-Host "Enter your Domain Controller's password" -AsPlainText -Force

Install-ADDSForest `
    -CreateDnsDelegation:$false `
    -DatabasePath "C:\Windows\NTDS" `
    -DomainMode "Win2016" `
    -DomainName $Domain `
    -DomainNetbiosName $NetbiosName `
    -ForestMode "Win2016" `
    -InstallDns:$true `
    -LogPath "C:\Windows\NTDS" `
    -NoRebootOnCompletion:$false `
    -SysvolPath "C:\Windows\SYSVOL" `
    -Force:$true `
    -SafeModeAdministratorPassword $safeModeAdminPassword
```

This script assumes how you would like to install the ADDS Forest. This is the only script I did not test as I promoted Windows Server to a Domain Controller through the Server Manager before attempting this. I'll need to double-check it once I create my single AD creation script in a future blog post!

<br>

<br>

## **3. Configure the AD forest and domain structure.** 

<br>

This step is highly repeatative and customizable which will make a great PowerShell script. However, I'll first explain how to do it through the Windows Server GUI. Lets first take a look at the default schema that is currently set up.

*  In the windows start menu, go to the Windows Start Menu in the bottem left of the screen or by clicking the windows key. 
* Then select the "Windows Administrative Tools" Folder.
* Select "Active Directory Users and Computers", also known as ADUC.
    
    Under your root domain, you can see the default setup. Although there is a Users folder, we will create our own Organizational Unit, or folder, for our users. 

    This is typically recommended for building out actual domains, and we want to hit relatively close to reality for our homelab.
*  To do this, go to select your root domain. Go to "Actions", then "New >", and then select Organization Unit (OU). 
* Name your OU. In my case, I named it "Yoshi Users" to differentiate it from the default "Users" folder.
* Repeat this process for the domain's Computers OU.
    
    Note: These two OUs will need to be redirected as the default Users and Computers OU. This is easier to do in PowerShell using the redircmp and redirusr commands. To use these you'll need to know the *distinguished name* of your new Users and Computers OU which can be found in the properties of those OUs.



<br>

Now that we understand how to create OUs, lets dive into some PowerShell scripts that can help with the creation of basic enterprise AD forest.

The most simple is the following script that will ask for OU name and distinguished name, or path, and create it for you.
``` PS
$Name = Read-Host "Enter the name of the OU"
$Path = Read-Host "Enter the path for the OU (e.g. 'OU=TestOU,DC=domain,DC=com')"
New-ADOrganizationalUnit -Name $Name -Path $Path
```
This is a nice simple use case for a single OU, but when AD is first installed, it'd be nice to have a PowerShell script that creates a basic forest that uses some best practices. Some best practices include having a Groups OU for permissions and separating Users and Computers OUs.

``` PowerShell
#Obtain domain and distinguished name info

$Domain = Read-Host "Enter the fully qualified domain name (FQDN) of the domain in which the forest will be created (e.g. yoshi.local):"
$FQDN = [System.Net.Dns]::GetHostByName($Domain).HostName
$FQDNArray = $FQDN.Split('.')
$HostName = $FQDNArray[0]
$TLD = $FQDNArray[1..($FQDNArray.Length-1)] -join '.'
$DomainShortName = Read-Host "What is the short-name for the domain or organization? (Example: If the organizations name is Yoshi Technologies LLC, type 'Yoshi'. This will create Yoshi Users, Yoshi Computers, Yoshi Servers, etc."
$Departments = @()

while ($true) {
    $Department = Read-Host "Enter the name of a department within your company and hit enter. Repeat this process for each department. Common ones include 'Marketing', 'Engineering', 'HR', 'Support'. When each department has been input, type 'exit'."

    if ($Department -eq 'exit') {
        break
    }

    $Departments += $Department
}

# Create the Users OU
New-ADOrganizationalUnit -Name "$DomainShortName Users" -Path "DC=$HostName,DC=$TLD"

# Create the Computers OU
New-ADOrganizationalUnit -Name "$DomainShortName Computers" -Path "DC=$HostName,DC=$TLD"

# Create the Servers OU
New-ADOrganizationalUnit -Name "$DomainShortName Servers" -Path "DC=$HostName,DC=$TLD"

# Create default server OUs
New-ADOrganizationalUnit -Name "DNS Servers" -Path "OU=$DomainShortName Servers,DC=$HostName,DC=$TLD"
New-ADOrganizationalUnit -Name "Email Servers" -Path "OU=$DomainShortName Servers,DC=$HostName,DC=$TLD"
New-ADOrganizationalUnit -Name "File Servers" -Path "OU=$DomainShortName Servers,DC=$HostName,DC=$TLD"
New-ADOrganizationalUnit -Name "DHCP Servers" -Path "OU=$DomainShortName Servers,DC=$HostName,DC=$TLD"

# Create the Groups OU
New-ADOrganizationalUnit -Name "$DomainShortName Groups" -Path "DC=$HostName,DC=$TLD"

# Create default group OUs
New-ADOrganizationalUnit -Name "Block PowerShell" -Path "OU=$DomainShortName Groups,DC=$HostName,DC=$TLD"
New-ADOrganizationalUnit -Name "Shared Folders" -Path "OU=$DomainShortName Groups,DC=$HostName,DC=$TLD"
New-ADOrganizationalUnit -Name "Security Cameras" -Path "OU=$DomainShortName Groups,DC=$HostName,DC=$TLD"

# Create OUs for each department
foreach ($Department in $Departments) {
    # Create the department Users OU
    New-ADOrganizationalUnit -Name "$Department" -Path "OU=$DomainShortName Users,DC=$HostName,DC=$TLD"

    # Create the department Computers OU
    New-ADOrganizationalUnit -Name "$Department" -Path "OU=$DomainShortName Computers,DC=$HostName,DC=$TLD"
}

# Create default computer OUs such as conference rooms, guest rooms, printers
New-ADOrganizationalUnit -Name "Conference" -Path "OU=$DomainShortName Computers,DC=$HostName,DC=$TLD"
New-ADOrganizationalUnit -Name "Storage" -Path "OU=$DomainShortName Computers,DC=$HostName,DC=$TLD"
New-ADOrganizationalUnit -Name "Printers" -Path "OU=$DomainShortName Computers,DC=$HostName,DC=$TLD"

#Change default OU for Computers and Users
redircmp "OU=$DomainShortName Computers,DC=$HostName,DC=$TLD"
redirusr "OU=$DomainShortName Users,DC=$HostName,DC=$TLD"
```
This script assumes that the ActiveDirectory module is installed on the system and that the account being used has permissions to create OUs in Active Directory.

If you are getting an error due to the redircmp or redirusr commands and would like to raise the domain level, use the following command. You can replace the $DesiredLevel variable with the desired functional level (e.g., Windows2000Forest, Windows2003InterimForest, Windows2003Forest, Windows2008Forest, Windows2008R2Forest, Windows2012Forest, Windows2012R2Forest, Windows2016Forest,).

``` powershell
#Raises the Domain Level to Windows2003Forest

$DesiredLevel = "Windows2003Forest"
$CurrentLevel = (Get-ADForest $Domain).ForestMode
if ($CurrentLevel -lt $DesiredLevel) {
    Set-ADForestMode -Identity $Domain -ForestMode $DesiredLevel
    Write-Output "Successfully raised domain level to $DesiredLevel"
}
else {
    Write-Output "The current domain level is already at or higher than $DesiredLevel"
}
```

<br>

<br>

## **4. Create user accounts for AD users.**
 
<br>

This will be done through the Active Directory Users and Computers (ADUC), which was accessed in the previous step. You can also access this by going to the "Tools" tab in the Server Manager. 

It's important to use a standardized naming convention for your users' usernames. For instance, a popular naming convention would be "john.doe" for someone named John Doe.

* In Active Directory Users and Computers, select your new OU for Users. In my instance, this is Yoshi Users.
* Go to Action > New > User.
* Fill out First Name, Last Name, and the Username.
* Click Next, and fill out the Password for the user to initially use to login. Make sure the correct boxes for your password policy are selected.
* Click Finish.

Here is a PowerShell script to create a user in Active Directory (AD) with input for First Name, Last Name, Username, and Password:

``` PowerShell
# Creates a New User in AD
$firstName = Read-Host "Enter the first name"
$lastName = Read-Host "Enter the last name"
$username = Read-Host "Enter the username"
$password = Read-Host "Enter the password" -AsSecureString

New-ADUser -GivenName $firstName -Surname $lastName -Name "$firstName $lastName" -SamAccountName $username -UserPrincipalName "$username@domain.com" -Path "OU=Users,DC=domain,DC=com" -AccountPassword $password -PasswordNeverExpires $false 
```

Note: Replace "domain.com" with your actual domain name and adjust the -Path parameter to match your AD structure. Also, make sure you have the necessary permissions to create a new user in AD.

<br>

However, since I am creating this AD forest for homelab use, I'll need a way to create a large amount of users at once. For this, I've made a PowerShell script that can do exactly that with the input of a single password, each end-user's full name, and the destination for these users. Unfortunately, this will only create bulk users for a specified destination.

``` PowerShell
Import-Module ActiveDirectory

#Ask the domain name and OU in order to create a variable for the correct path
$Domain = Read-Host "Enter the active directory domain in which the new users will be created (e.g. yoshi.local)"
$HostName = $Domain.Split(".")[0]
$TLD = $Domain.Split(".")[1]
$OU = Read-Host "Enter the Name of the department in which the Users will be created. (e.g. Accounting)"
$CapitalizedHostName = $HostName.Substring(0,1).ToUpper() + $HostName.Substring(1)
$UsersOU = "$CapitalizedHostName Users"
$OUPath = "OU=$OU,OU=$UsersOU,DC=$HostName,DC=$TLD"
if((Get-ADOrganizationalUnit -Filter {DistinguishedName -eq $OUPath})) {} else {
    $OUPath = Read-Host "The OU does not exist for that department. Enter the path in which the Users will be created. (e.g. OU=Accounting,DC=yoshi,DC=local)"
}

#Ask user for the default password for the new users and for the list of new users names
$Password = Read-Host -AsSecureString "Enter the default password for all new users"
$ListOfNames = Read-Host "Enter the list of full names for the new users. (e.g. John Doe, Spongebob Squarepants, Arnold Schwarzenegger)"
$FullNames = $ListOfNames.Split(',')

foreach ($FullName in $FullNames) {
    $FirstName, $LastName = $FullName.Trim().Split(' ')
    $UserName = "$FirstName.$LastName"
    $Email = "$UserName@$Domain"
        New-ADUser `
        -Name $FullName `
        -UserPrincipalName $Email `
        -SamAccountName $UserName `
        -GivenName $FirstName `
        -Surname $LastName `
        -DisplayName $UserName `
        -AccountPassword ($Password) `
        -ChangePasswordAtLogon $True `
        -Path $OUPath
        -Enabled $True
}
```

- Assumes OU for Users is formatted as follows: "HostName Users"
- Assumes the OUs within Users are their listed departments: "Accounting" "Engineering"

<br>

### **Adding Users to the Domain Admin Group**

After having a sufficient amount of users for the local domain, we will now need to create a domain admin. Although the default domain admin exists, it's best to create a new one and disable the default for security best practicies.

In order to do this with one of the existing users:
* Double-click the user in ADUC.
* Select the "Member of" tab.
* Select "Add..."
* In the "Enter the object names to select" field, type "Domain Admin" and hit enter.
* This should add the "Domain Admins" group. Select "Ok" to save the changes.

You can also use the following PowerShell script:

``` PowerShell
$Username = Read-Host -Prompt "Enter the username of the AD user you want to add permissions to. (e.g. John.Cena)"
$User = Get-ADUser -Filter "SamAccountName -eq '$Username'" -ErrorAction SilentlyContinue
if ($User -eq $null)
{
    Write-Host "Error: The user $Username does not exist in Active Directory."
    return
}
$DomainAdmins = Get-ADGroup -Filter "Name -eq 'Domain Admins'"
Add-ADPrincipalGroupMembership -Identity $User.DistinguishedName -MemberOf $DomainAdmins.DistinguishedName
Write-Host "Success: The user $Username has been added to the Domain Admins group."
```

<br>

<br>

## **5. Domain Join Your Computers.**

<br>

In order to join a computer to the domain, I'll be creating a Windows 10 VM in Proxmox. Once the OS is installed and the computer is booted, the first step to domain joining it is to redirect the DNS to your domain controller's IP address. 

In order to change DNS to your domain controller's IP through the Windows GUI:
* Open Network & Internet Settings in Windows by right clicking the ethernet icon in the bottom right of the Windows Taskbar.
* You should be on the "Status" page. Select "Properties" under the Ethernet Instance.
* Under "IP settings" select "Edit".
* In the "preferred DNS" field, paste your domain controller's IP address. This can be found by using the powershell command "ipconfig" on the domain controller.

<br>

The next step will be to change the name and domain of the computer. Prior to these steps, you must have a Domain Admin created in ADUC and know the credentials.
* Go to "About your PC". You can search for this in the Windows Search Bar.
* Under "Related settings", select "Rename this PC (advanced)"
* Select "Change" next to "To rename this computer or change its domain or workgroup, click Change."
* Here you'll change the computer's Name. (The following naming conventions that I'll be using )
* Then select "Domain:" under "Member of" and input the AD domain.
* Click "Ok" and then enter the credentials to a valid domain user.

### **Computer Naming Convention:**

For best practice, I will be using the following format:

Type - department or location code - asset #

    Type
        W = Workstation
        L = Laptop
        P = Printer
        S = Server
        V= VDI or Virtual Machine
    Department: Use two letter appreciations for departments or use a location code
        HR = Human Resources
        AC = Accounting
        MR = Marketing
        EN = Engineering
        IT = Support

Using this naming convention, my first new computer name will be V-IT-0001.

I'll be using the following PowerShell script on already-created VMs that I wish to domain join, just to keep from having to do the latter process.

``` powershell
#Change the DNS server to the IP of the Domain Controller
$DNSServer = Read-Host "Enter the IP address of the Domain Controller"
$NetworkAdapter = Get-NetAdapter | Where-Object {$_.Status -eq "Up"}
$NetworkAdapter | Set-DnsClientServerAddress -ServerAddresses $DNSServer
Write-Host "Primary DNS server has been set to $DNSServer"

#Change the Computer Name to 
$newComputerName = Read-Host "Enter the new computer name using the correct naming convention. (e.g. V-HR-0025)"
$domainName = Read-Host "Enter the domain name. (e.g. example.com)"
$credential = Get-Credential

Rename-Computer -NewName $newComputerName
Add-Computer -DomainName $domainName -Credential $credential -restart
```

Since I used an old VM to domain join, I wasn't able to join it during the installation. If installing a fresh version of Windows, you can have it domain joined before it's logged in by entering the domain during install.

<br>

<br>

## **6. Create and assign default Group Policy Objects.**

<br>

There are many GPOs that are good to use for best practice. For this step, I'll just be showing the GPO that will block PowerShell to any linked-OUs. Other GPOs that are common are as follows:

* Prevent access to the command prompt
* Deny all removable storage access
* Disable Control Panel Access
* Disable Prohibitted Software Installation
* Disable Guest User Accounts for all Computers
* Prevent forced restarts for logged-on Users
* Monitor changes to your GPO settings
* Conference Room Computers Screen Timeout


**How to create a GPO that prevents users from using PowerShell:**

* Go to the "Group Policy Management" tool in Windows Server.
    This should look very familiar to ADUC but with the addition of a few new OUs.
* Select the "Group Policy Objects" OU, right click, and select "New".
    * Alternatively, Right-click the OU you wish to create a GPO for and select "Create a GPO in this domain, and Link it here..."
* Give the GPO a name and click "Ok". Try to be very descriptive. I'll name mine "User - Disable PowerShell".
* Right click the newly created GPO and select "Edit". This is where we'll add the specific rule for a GPO.
* Navigate to User Configuration > Policies > Windows Settings > Security Settings > Software Restrictrion Policies > Additional Rules
* Now, right-click "Software Restriction Policies" and select "New Software Restriction Policies".
* Select "Additional Rules", then right-click and select "New Path Rule".
* Now you'll need to input the location of powershell.exe in the "Path" field. The most common path for PowerShell is C:\Windows\System32\WindowsPowerShell\v1.0 but it can also be found using Task Manager.
* Set the "Security Level" to “Disallowed”. Click OK.
    * Now, if you still need to link the GPO, navigate back to the Group Policy Management tool.
    * Right-Click the OU you wish to apply the GPO to and select "Link an existing GPO...". 
    * Select your newly created and edited GPO and click "Ok". 

It turns out creating GPOs via PowerShell doesn't really exit as PowerShell lacks the commands to do so. You can make PowerShell scripts create GPOs that edit registry keys, but it's not recommended since editing registry keys can be very damaging to a system if you're not sure what you're doing. Here's the script that will have the same effect, but do it by editing registry keys:

``` PowerShell
New-GPO -Name "Prevent PowerShell Usage"
Set-GPRegistryValue -Name "Prevent PowerShell Usage" -Key "HKCU\Software\Microsoft\Windows\CurrentVersion\Policies\System" -ValueName "ExecutionPolicy" -Restricted
```
I'll look forward to the day in which I can create GPOs through PowerShell scripts without having to edit registry keys, but that seems too convenient for hackers. I guess key registry or manual input will have to do for now...


<br>

<br>

<br>

## **Closing Thoughts**

<br>

It was nice to to use Active Directory to start learning about PowerShell scripting. I enjoyed the head scratches when it came to the scripts not working like I thought they would. I'll need to combine all the scripts into one that builds me one fully functional and practical AD forest. However, I'll save that endeavour for another post. If you made it this far, thanks for reading and Happy Active Directorying!

<br>

<br>

<br>