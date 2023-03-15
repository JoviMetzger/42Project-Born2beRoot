
# Born2beRoot Project is about setting up your first server

## Table of Contents

- [Basic Information](#Basic-Information)
- [Step By Step](#Step-By-Step)
	- [1. Download](#1.-Download-Debian-or-Rocky-*(Debian-is-easier-to-install-&-since-it-is-recommended-for-new-users-and-small-or-personal-servers)*)
	- [2. Open VisualBox](#2-Open-VisualBox)
	- [3. Installing Sudo](#3-Installing-Sudo)
	- [4. Installing and Configuring SSH](#4-Installing-and-Configuring-SSH)
	- [5. Changing Default Port](#5-Changing-Default-Port(22)-to-4242)
	- [6. Installing and Configuring UFW](#6-Installing-and-Configuring-UFW(Uncomplicated-Firewall))
	- [7. Setting Up a Strong Password Policy](#7-Setting-Up-a-Strong-Password-Policy)
	- [8. Create Group & Add User](#8-Create-Group-&-Add-User)
	- [9. Configuring Sudoers Group](#9-Configuring-Sudoers-Group)
	- [10. Monitoring Script](#10-Monitoring-Script)
	- [11. Take a Snapshot](#11-Take-a-Snapshot-*(so-that-your-signature-doesn't-change)*)
- [Extra Commands](#Extra-Commands)

## Basic Information
- **Operating system:** The operating system is a lower-level software that manages computer hardware and provides basic functions, such as scheduling tasks. It allows users to interact with the computer without having to know how to speak a computer language. Examples of operating systems include Microsoft Windows, macOS, and Android.

- **VirtualBox:** VirtualBox is a software-based computing resource that runs programs and deploys apps. It allows one or more virtual "guest" machines to run on a physical "host" machine. Each virtual machine runs its own operating system and functions independently of other virtual machines, even when running on the same host. Essentially, it's like having a mini computer inside your computer, and any mistakes made within VirtualBox won't affect your main system.

- **Purpose of a virtual machine:** Virtual machines are deployed for various reasons, such as accommodating different levels of processing power needs, running software that requires a different operating system, or testing applications in a safe, sandboxed environment.

- **Aptitude & apt:** Aptitude is a high-level package manager that automatically removes unused packages, allows users to check which packages are required and which may conflict with an installation, and suggests alternatives if conflicts arise. Apt is a low-level package manager that can be used by other high-level packages.

- **AppArmor:** AppArmor is a security system that provides tools to isolate applications from each other. This helps to isolate an attacker from the rest of the system in the event that an application is compromised.

- **SSH:** SSH, or Secure Shell, is a network protocol that provides a secure way for users, particularly system administrators, to access a computer over an unsecured network.

- **LVM:** LVM stands for Logical Volume Management. It is an advanced and flexible system for managing logical filesystems that is more advanced and flexible than traditional methods of partitioning a disk into one or more segments and formatting those partitions with a filesystem.

- **Partitions:** A partition is a section of a hard drive that is separated from other segments. Partitions enable users to divide a physical disk into logical sections, allowing multiple operating systems to run on the same device.

- **Sudo:** Sudo allows users to run programs with the security privileges of another user. It prompts users for their personal password and confirms their request to execute a command by checking a file called sudoers, which the system administrator configures.

- **Uncomplicated Firewall (UFW):** UFW is the default firewall configuration tool for Ubuntu. Developed to ease iptables firewall configuration, UFW provides a user-friendly way to create an IPv4 or IPv6 host-based firewall.

- **Cron:** Cron is a Linux command used for scheduling tasks to be executed at a future time. It is commonly used to schedule jobs that are executed periodically, such as sending out a notice every morning.

- **Require TTY:** The "requiretty" option means that non-root code won't be able to directly upgrade its privileges by running sudo. This provides an additional layer of security in the event that some non-root code is exploited, such as a PHP script.


## Step By Step
- ### 1. Download Debian or Rocky *(Debian is easier to install & since it is recommended for new users and small or personal servers)*
- ### 2. Open VisualBox
    - #### 1.New
		- Fill in: 
			- name *(name of project)*
			- machine folder *(leave as it is)*
			- type *(linux)*
			- version *(debian (64-bit))*
		- memory size **->** continue **->** continue **->** continue **->** continue **->** create
	- #### 2. Setting
		- storage
		- click on empty
		- optical driven:  
			- right click on blue button 
            - select choose a disk file... 
            - choose the debian that yoy downloaded earlier
	- #### 3. Start
		- Install
		- English
		- united states
		- american english
		- hostname: *intra login + 42 (login42)*
		- domain name: enter/continoue
		- rootpassword
		- full name of user
		- username: *intra login + 42 (login42)*
		- user password
		- central
		- guided: use entire disk and set up encrypted LVM
		- SCSI3 ...: **enter**
		- the erasing data on SCSI3 ... Page: **PRESS CANCEL**
		- encryption passphrase
		- amount of volume: **enter max**
		- at scan another CD or DVD: **No**
		- debian archive mirror county: *Choose a country*
		- deb.debian.org
		- HTTP proxy infomation (blank for none): enter/continoue
		- choose software to install: Deselect "SSH Server" *(to deselect, just hit SPACE key)*
		- /dev/sda (ata.....): **Not manually**

	- #### 4. After login
		check your first image on the PDF *(this is your partition)*: *Doesn't need to be intenicall, it just needs* **->** *root,home,swap_1*
		```bash
		$ lsblk
		```
											
- ### 3. Installing Sudo
    - #### 1. Login as root
		```bash
		$ su
		```
	- #### 2. Install sudo
		```bash
		# apt-get update
		# apt-get upgrade
		# apt install sudo
		```
	- #### 3. Adding user in sudo group
		```bash
		# usermod -aG sudo <your_username>
		```
	 	*exit: to go out of root*
	- #### 4. Check if user is in sudo group
		```bash
		 getent group sudo
		```
		*if not:*
		```bash
		$ sudo adduser <username> sudo
		```
		*check again*
	- #### 5. Open sudoers file:
		```bash
		$ sudo whoami
		```
		*it should say "root" (if not):*
		```bash
		$ sudo visudo
		```
		*add this line in file:* 
		```bash
		your_username    ALL=(ALL:ALL) ALL
		```
	
- ### 4. Installing and Configuring SSH
	- #### 1. Update, Upgrade & Install 
		```bash
		$ sudo apt update
		$ sudo apt upgrade
		$ sudo apt install openssh-server
		```
		
	- #### 2. Check the SSH server status
		```bash
		$ sudo systemctl status ssh
		```
	- #### 3. Restart the SSH service
		```bash
		$ service ssh restart
		```

- ### 5. Changing Default Port(22) to 4242
	- #### 1. Open file with port in it
		```bash
		$ sudo nano /etc/ssh/sshd_config
		```
	- #### 2. Edit the file change the line #Port22 to Port 4242
		Find line and change:
		```bash
		#Port 22		->		Port 4242  (without the #)
		```
	- #### 3. Check if port settings got right
		```bash
		$ sudo grep Port /etc/ssh/sshd_config
		```
	- #### 4. Connecting SSH server
		- Add forward rule for VirtualBox
			- 1.Go to VirtualBox **->** Choose the VM **->** Select Settings
			- 2.Choose “Network” **->** “Adapter 1" **->** ”Advanced” **->** ”Port Forwarding”
			- 3.Add 
				- Enter value 4242 to Host Part & Guest port
		- Restart SSH server 
		```bash
		$ sudo systemctl restart ssh
		```
		- Check ssh status:
		```bash
		$ sudo service ssh status
		```
		- Connect in the host terminal
		```bash
		$ ssh <your_username>@localhost -p 4242
		$ ssh <username>@127.0.0.1 -p 4242
		```
		- If you want to quit the connection
		```bash
		$ exit
		```

- ### 6. Installing and Configuring UFW (Uncomplicated Firewall)
	- Install UFW
	 	```bash
		$ sudo apt install ufw
		```
	- Enable
		```bash
		$ sudo ufw enable
		```
	- Check the status
		```bash
		$ sudo ufw status verbose
		```
	- Configure the port rules
		```bash
		$ sudo ufw allow <Port> (4242)
		```
	- Add port *(8080)*
		```bash
		$ sudo ufw allow <port_number>
		$ sudo ufw status numbered
		```
	- Delete Port 8080
		```bash
		$ sudo ufw delete <number>
		```

- ### 7. Setting Up a Strong Password Policy
	- #### 1. Configure password age policy 
		```bash
		$ sudo /etc/login.defs
		```
		- To set password to expire every 30 days
			- replace line:
			```bash
			160 PASS_MAX_DAYS 99999 	->		with: 160 PASS_MAX_DAYS 30
			```
		- To set minimum number of days between password changes to 2 days
			- replace line:
			```bash
			161 PASS_MIN_DAYS 0 	->		with: 161 PASS_MIN_DAYS 2
			```
		- To send user a warning message 7 days *(defaults to 7 anyway)* before password expiry
			- keep below line as is
			```bash
			162 PASS_WARN_AGE 7
			```
	- #### 2. Password Strength
		- Install the libpam-pwquality package
			```bash
			$ sudo apt install libpam-pwquality
			```
		- Verify whether libpam-pwquality was successfully installed
			```bash
			$ dpkg -l | grep libpam-pwquality
			```
		- Configure password strength policy 
			```bash
			$ sudo nano /etc/pam.d/common-password
			```
		- Add after:
			```bash
			<~~~>
			25 password requisite pam_pwquality.so retry=3
			<~~~>
			```
            - minlen=10 **->** *To set password minimum length to 10 characters*
			- ucredit=-1 dcredit=-1 lcredit=-1 **->** *To require password to contain at least an uppercase character and a numeric character*
			- maxrepeat=3 **->** *To set a maximum of 3 consecutive identical characters*
			- reject_username **->** *To reject the password if it contains <username> in some form*
			- difok=7 **->** *To set the number of changes required in the new password from the old password to 7*
			- enforce_for_root **->** *To implement the same policy on root*
			- **It should look like the below:**
				```bash
				pam_pwquality.so retry=3 minlen=10 ucredit=-1 dcredit=-1 lcredit=-1 maxrepeat=3 reject_username difok=7 enforce_for_root
				```
		- Reboot the change affects
			```bash
			$ sudo reboot
			```
		- IMPORTANT!!! you need to change your user password condision aswell **->** manually 
        - Change user password condisions
			```bash
			$ sudo chage -M 30 <username>
			$ sudo chage -m 2 <username>
			$ sudo chage -W 7 <username>
			```
        - Check if it worked
			```bash
			$ chage -l <username>
			```
- ### 8. Create Group & Add User
	- Create a group
		```bash
		$ sudo groupadd <username>
		```
	- Check if group created
		```bash
		$ getent group
		```
	- Assign user to group
		```bash
		$ sudo adduser <username> <group>
		```
	- Check if the user is in group
		```bash
		$ getent group <username>
		```
						
- ### 9. Configuring Sudoers Group
	- Create sudoers.d
		```bash
		$ sudo touch /etc/sudoers.d/sudoconfig
		```
	- You need to create your /var/log/sudo file
		```bash
		$ sudo mkdir /var/log/sudo
		```
	- Verify /var/log/sudo/ file
		```bash
		$ su 
		$ cd /var/log/sudo/00/00
		$ ls
		$ sudo ls		->		run a sudo ls to see if the folder is updated
		```
	- Go to sudoconfig file
		```bash
		$ sudo nano /etc/sudoers.d/sudoconfig
		```
	- Add following for authentication using sudo
		```bash
		Defaults   	secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
		Defaults    passwd_tries
		Defaults    badpass_message="Password is wrong, please try again!"
		Defaults	logfile="/var/log/sudo/sudo.log"
		Defaults	log_input,log_output
		Defaults	requiretty
		```
    - IMPORTANT!!! you need to change your paaword of root and user aswell (PDF says so)
		```bash
		$ sudo passwd <username>		<- changes root password
		$ passwd						<- changes user password
				
		```
	
				
- ### 10. Monitoring Script
	- Install net-tools for the crontab
		```bash
		$ sudo apt install net-tools
		```
	- Create the "monitoring.sh."   
		- ***NOTE:**  It must be developed in bash (#!/bin/bash)*
	- Create like the giving example the Architecure, CPU physical ..., and tell it what it is surpossed to display after each word.
	- **Like this:**
		```bash
		#!/bin/bash
				ARC=$(uname -a)
				CPUP=$(nproc --all)
				VCPU=$(cat /proc/cpuinfo | grep processor | wc -l)
				RAM=$(free -m | awk '$1 == "Mem:" {print $2}')
				FRAM=$(free -m | awk '$1 == "Mem:" {print $3}')
				PRAM=$(free | awk '$1 == "Mem:" {printf("%.2f"), $3/$2*100}')
				DISK=$(df -Bg | grep '^/dev/' | grep -v '/boot$' | awk '{ft += $2} END {print ft}')
				FDISK=$(df -Bm | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} END {print ut}')
				PDISK=$(df -Bm | grep '^/dev/' | grep -v '/boot$' | awk '{ut += $3} {ft+= $2} END {printf("%d"), ut/ft*100}')
				CPUL=$(top -bn1 | grep '^%Cpu' | cut -c 9- | xargs | awk '{printf("%.1f%%"), $1 + $3}')
				LB=$(who -b | awk '$1 == "system" {print $3 " " $4}')
				lvmt=$(lsblk | grep "lvm" | wc -l)
				LVM=$(if [ $lvmt -eq 0 ]; then echo no; else echo yes; fi)
				CTCP=$(cat /proc/net/sockstat{,6} | awk '$1 == "TCP:" {print $3}')
				LOG=$(users | wc -w)
				IP=$(hostname -I)
				MAC=$(ip link show | awk '$1 == "link/ether" {print $2}')
				SUDO=$(journalctl _COMM=sudo | grep COMMAND | wc -l)
				wall "#Architecture: $ARC
	 				  #CPU physical: $CPUP
					  #vCPU: $VCPU
					  #Memory Usage: $RAM/${FRAM}MB ($PRAM%)
	 				  #Disk Usage: $DISK/${FDISK}Gb ($PDISK%)
					  #CPU load: $CPUL
					  #Last boot: $LB
					  #LVM use: $LVM
					  #Connexions TCP: $CTCP ESTABLISHED
					  #User log: $LOG
					  #Network: IP $IP ($MAC)
					  #Sudo: $SUDO cmd" 
		```
	- Give script execution permission
		```bash
		$ chmod 775 monitoring.sh
		```
	- Reboot
		```bash
		$ sudo reboot
		```
	- Execute the script
		```bash
		$ sudo /usr/local/bin/monitoring.sh
		```
	- Open crontab and add the rule *(BUT make sure you add this in the root and not as user)*
		```bash
		$ sudo crontab -e 
		```
		OR
		```bash
		# crontab -e
		```
	- To schedule a shell script to run every 10 minutes
		- replace line
		```bash
		# m h dom mon dow command		->		*/10 * * * * bash /usr/local/bin/monitoring.sh 
		```
	- Script runs every 1 minute 
		```bash
		*/1 * * * * bash /usr/local/bin/monitoring.sh 
		```
	- Script Stop running **->** just command line out *(#)*
		```bash
		# */1 * * * * bash /usr/local/bin/monitoring.sh
		```
- ### 11. Take a Snapshot *(so that your signature doesn't change)*
	- Go to your virtualbox and click on the 3 lines next to your project, snapshot should come up
	- Click on 'snapshot' and 'take' *(should then say Snapshot1)*
	- Copy & Paste signature of your machine’s virtual disk in signature.txt
	- How to create a signature:
		- Inside your virtualBox folder use command to create signature
			```bash
			$ shasum <myVMname>.vdi
			```
		- HINT: it takes a while to load, you know it worked if a line with bunch of numbers appears.
	
## Extra Commands
- #### 1. Change Hostname
	- Check current hostname
		```bash
		$ hostnamectl
		```
	- Change Hostname
		```bash
		$ sudo suhostnamectl set-hostname <new>
		```
	- Reboot and check change 
		```bash
		$ sudo reboot
		```
- #### 2. Other Commands
	- Checks all users in machine
		```bash
		$ cut -d: -f1 /etc/passwd
		```
	- Checks which group my user belongs to
		```bash
		$ groups
		```
	- Checks the status of AppArmor
		```bash
		$ sudo aa-status
		```
	- Delete group
		```bash
		$ sudo groupdel
		```

- #### 3. Monitoring script notes
	- ***uname:** architecture information*
	- ***/proc/cpuinfo:** CPU information*
	- ***free:** RAM information*
	- ***df:** disk information*
	- ***top -bn1:** process information*
	- ***who:** boot and connected user information*
	- ***lsblk:** partition and LVM information*
	- ***/proc/net/sockstat:** TCP information*
	- ***hostname:** hostname and IP information*
	- ***ip link show / ip address:** IP and MAC information*


