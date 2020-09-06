# TryHackMe-kenobi

Task 3:
  #1 	Lets get the version of ProFtpd. Use netcat to connect to the machine on the FTP port.

  What is the version? Ans: 1.3.5  #nc ip port
 
  	
#2 We can use searchsploit to find exploits for a particular software version.

Searchsploit is basically just a command line search tool for exploit-db.com.

How many exploits are there for the ProFTPd running?

"searchsploit ftp | grep 1.3.5"

#3You should have found an exploit from ProFtpd's mod_copy module. 

The mod_copy module implements SITE CPFR and SITE CPTO commands, which can be used to copy files/directories from one place to another on the server. Any unauthenticated client can leverage these commands to copy files from any part of the filesystem to a chosen destination.

We know that the FTP service is running as the Kenobi user (from the file on the share) and an ssh key is generated for that user. 

#4	
We're now going to copy Kenobi's private key using SITE CPFR and SITE CPTO commands.


#
#5	
Lets mount the /var/tmp directory to our machine
sudo mount <IP> :/var /temp/kenobi ( this only applicable because SITE CPFR-SITE CPTO)

#6 "cp" command to use id_ras/ "ssh -i id_rsa kenobi@<IP>" 

Task 4:
  #1 find / -perm -4000 -type f 2>/dev/null " OR " find / -perm -u=s -type f 2>/dev/null
  
  #3 	
Strings is a command on Linux that looks for human readable strings on a binary.



This shows us the binary is running without a full path (e.g. not using /usr/bin/curl or /usr/bin/uname).

As this file runs as the root users privileges, we can manipulate our path gain a root shell.



We copied the /bin/sh shell, called it curl, gave it the correct permissions and then put its location in our path. This meant that when the /usr/bin/menu binary was run, its using our path variable to find the "curl" binary.. Which is actually a version of /usr/sh, as well as this file being run as root it runs our shell as root!

(This is a very command SUID linux priv escalate. Find a SUID binary and change the PATH variable to make system look for our "shell" with the same name of that oringal binary file.  


    
