# Enterprise_Network_Design
 Enterpise network  Designs for Corporations with Packet-Race Efficiency Optimization
 
 Objectives:
Through this assignment, you should demonstrate your ability in:
•	setup a Domain Controlled computer system,
•	managing AD Users and Group permission
•	Domain Policy configuration,
•	registry configuration,
•	regular expressions,
•	automate the administrative tasks using PowerShell scripts.

Pre-requisite:


	Domain controller
WinServer 2019	Trainee Computers
Windows 7/8.1/10	Trainer Computers
Windows 7/8.1/10
Domain	OnlineZXX.hk*	OnlineZXX.hk*	OnlineZXX.hk*
Computer Name	OnlineDC	Lab1-s01…Lab1-s15
Lab2-s01…Lab2-s15	Lab1-t01
Lab2-t02
IP Address	192.168.20.1	192.168.20.11 to
192.168.20.40	192.168.20.6 and
192.168.20.7
Subnet Mask	255.255.255.0	255.255.255.0	255.255.255.0
Default Gateway	192.168.20.254	192.168.20.254	192.168.20.254
Preferred DNS	192.168.20.1	192.168.20.1	192.168.20.1
	
* In the domain name “Onlineabc.hk”, abc represents your group and sequence inside the group.  E.g. If you are the fourth student in group Z, you should set the domain name to “OnlineZXX.hk”. 
XX mean the last digit of student ID If your campus has provided a Domain.xlsx file, please follow the domain assigned in that file.  

Scenario

You are the IT administrator of a professional trainer center named “Online”. And you have received the request from your boss about some accounts, server and desktops setup in order to facilitate the learning of trainee.

The following are the requirements of the configurations:

1.	All the computers in the departments will be managed by a domain controller. Windows Server 2019 R2 is employed to set this domain controller (DC).
2.	There are 2 computer laboratories in the center, namely Lab1 to Lab2 Each laboratory has 16 computers in total, 15 for the trainees and 1 for trainer. The operating system of the computers in the laboratories are Windows 7/8/10. The names of the computers are assigned in the format of “Labφ-Sψψ” where ψψ is from 01 to 15, and φ is from 1 to 2 representing the lab number. The name of the trainer machine is “LABφ-T01”.  You will manually join the desktop computer to the domain managed by the DC. (Yet, you are required to generate the rest of computer accounts in the DC because you will not really setup all 32 Windows workstation VM in this assignment.)
3.	AD account will be created for each trainee/trainer according to information of the trainees and trainers that is provided.  Two files of the users are provided for initial setting of the users. Intake20.csv is the information of the trainees in comma-separated-values format, while the Trainers.txt is the information of the trainers in text format.
a.	All the trainees can login to all computers in the laboratories except trainer’s computer. I.e. For security reason, trainees are NOT allowed to login to any trainer’s computer. Yet, trainers can login to any computer in the laboratories. 
b.	All the users using roaming profile and that is stored in the DC.
c.	Creating Trainee’s accounts from the Intake20.csv: 
i.	The login ID has already been provided in csv and that will be used as the  Logon Name of the Domain account.
ii.	The Intake20.csv may contains some incorrect information.  You are required to extract the required information from the file (regular expression may help) and create the corresponding AD accounts. 
iii.	The first name, last name, email address and phone number should be filled in the corresponding field of the account.
1.	The password need to follow the policy of minimum 8 characters with at least 1 numeric and 1 special character. 
2.	The email address must be a valid one. 
3.	The phone number must be a valid one. (i.e. eight digit for HK despite the 852 area number)
iv.	For easier management, a group called “Trainees” will be created for the trainees. 
v.	For users with incorrect phone number, password or email address, their user accounts will still be created but disabled. The usernames of these disabled users should be recorded in the corresponding error log files named: InvalidPassword.txt, InvalidEmail.txt, and InvalidPhone.txt.  The administrative staff will follow up to clarify the cases manually.
d.	Creating Trainer’s accounts from the Trainers.txt. 
i.	Regarding the trainer accounts, the login name is already provided in the text file. 
ii.	The last name and HKID number of the trainer should be used to form the initial password, e.g. in the first record, the tutor is Chan Tai Man and his HKID is B657474(3), the default Password will be “chan$B657474” without the quote.  Note that the first alphabet of the HKID number is in uppercase, the check digit together with the brackets are neglected, and the last name is in lowercase. 
iii.	Other information such as Last Name, First Name, Telephone Number will also be extracted and filled in the corresponding field of the AD User accounts
iv.	For easier management, a group called “OnlineTrainer” will be created for the trainers. 

4.	The DC should provide a network share so that each user has his/her home folder to store his/her personal files.

a.	All trainees and trainers will be assigned an individual share folder in \\OnlineDC\personal\%username%\ where only the corresponding user has the full access rights and all other users (except those in Administrators group) will NOT have any access permission.  The shared folder is automatically mapped as F: drive after the user login and each user will have a quota of 8GB at a warning level of 6GB.  

b. All trainers will have an additional network share folder for collecting files at \\OnlineDC\DropAndPick\%username%\ where ALL users will be able to put files in it but they cannot not read files uploaded by the other users.  Only the corresponding owner of the DropAndPicker folder will have the permission to read and write as well as delete files in it. The network share \\OnlineDC\DropAndPick will be automatically mapped as G: drive for ALL the users (including the trainees). Each trainer will have a quota of 40GB usage.
 

5.	Regarding the workstation security,
a.	the following security policy/registry tweaks will be applied on the “trainee accounts”. 
i.	Registry editor” should NOT be accessible. 
ii.	“Task manager” should NOT be accessible. 
iii.	“Command prompt” should NOT be accessible. 
b.	the following security policy/registry tweaks are applied on ALL accounts. 
i.	Add Encrypt / Decrypt Options to Windows Right-Click Menu
ii.	Accounts will be suspended for 30minutes after 3 times wrong password entry.

Deliverables

I.	An archive (.zip or .7z) is submitted to Moodle on or before the deadline containing the following:

	i)	A written configuration report (.doc or .docx) on the overall setup plan of the system, especially the manual setup of the DC & desktop computers, and NTFS & share permissions setup of the directories should be provided.

		You should have at least 1 DC, 1 Trainee’s computer and 1 Trainer’s computer for testing.

		To provide evidence of your work, screen captures with descriptions should be provided in the report for all the critical setup of server and client computers.  Hints: In some of the appropriate steps, we expect you will mention the usages of the scripts that you are required to submit in point ii) below. 

	ii)	The parts highlighted above in red (i.e. from last part of point 2 to point 5) would normally be automated using PowerShell script. Script files (.ps1) for all the automation tasks need to be submitted.  For easier identification of your scripts, the name of the script should be named properly with the task you are working with. (e.g. the script file of the Task 3c should be named “3c_SetupTraineeAccount.ps1”. ) (Note: You may use more than one script to accomplish a task.  Besides, sufficient inline documentation is a good practice.) 

	
 
	iii)	MD5 checksum(s) of your VMs. 
		You should have created and configured at least 1 DC, 1 Trainer’s computer and 1 Trainee’s VMs in this assignment (Note: You can prepare more VMs if necessary.).  In some cases, we may require you to provide the VMs for further checking.  To ensure you have not changed the VMs after the submission deadline, you are required to make MD5 checksums of your VMs.  Before calculating the MD5 checksum, you should compress all VM files into one file (or one file for each VM).  Then, PC users can use MD5checker (http://getmd5checker.com/download) for calculating the checksum. MacOS users can use the built-in command “md5” for the same purpose.  Please put the MD5 checksums of your VMs in a separate file named Checksum.txt.
	
	iv)	Record videos to demonstrate all the details configurations of your system setup.  Provide a download link of Google drive at the end of your configuration report if the size of the videos clips is greater than 1GB.  The videos must include but not only limit to:
a.	All the manual configuration of the domain controller. 
b.	Computers and Users accounts details. (just need to include 1 trainers and 2 trainee in your demo)
c.	Permission of the all the directories, profiles.
d.	Demonstrating that the roaming profile is working for trainers and trainees.
e.	Quotas of shared directories.
f.	Trainees cannot logon to trainer’s machines but trainers can.
g.	The mapped drive F: and G:
h.	Registry tweaks.

 

