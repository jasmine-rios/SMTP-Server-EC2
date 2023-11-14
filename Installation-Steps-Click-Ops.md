# Installation-Steps-Click-Ops

## Obtain a domain

You can obtain a domain to try this from domain sellers like Godaddy, AWS Route 53, and others.

I have a domain as I have for a personal project
## Create EC2 instance

1. In AWS go to EC2 and launch an instance.

2. Choose the AMI as CentOS 7 and use t2.micro as it will be test. Keep the rest default and download key. Create the instance.

3. In the security group for the instance add the following inbound rules: (This eventually will get locked down so it does not come from anywhere as it is a security concern)

- SSH, custom, your IP (Find public IP from doing this in terminal: `curl icanhazip.com`)

- SMTP from anywhere

- SMTPS from anywhere

- Port 587 from anywhere (SMTP via TLS)

- Port 2525 from anywhere (Alternate SMTP port)

## Set Postfix on Instance

1. SSH into instance in terminal

`ssh -i <name of key> centos@<public IP address>`

Type yes when prompted to save host.

2. Now that you are in the centos instance do the following: 
(HINT: You could have also done this in the user data before launching instance if you are going to create multiple instances)

```bash
# Enter Sudo

sudo -i

# Install postfix, vim, telnet

yum install postfix

yum install vim

yum install telnet

# edit the postfix main.cf file 
# Remember that i let's you change the file
vim /etc/postfix/main.cf
# Uncomment the following and add these changes near line 75
myhostname = smtp.example.local (Need to Verify)

mydomain = <insert your domain name> (Need to Verify)
# Uncomment the following near line 99
myorigin = $mydomain

# Make sure it's uncommened and set ipv4 near line 116

inet_interfaces = <private ip4 from instance>

# Make sure it's uncommented near line 119

inet_protocols = all

# Make sure it's uncommented near line 164

my destination = $myhostname, localhost.$mydomain, localhost

# Uncomment and add IP range for Public and Private IPs from AWS near line 264

mynetworks = <public IP/CIDR> <Private IP/CIDR> (Need to Verify)

# Uncomment this near line 419
home_mainbox = Maildir/

# Save and Exit out of Vim.
# To save your work use escape key then type :x
```

3. Now that the postfix file is configured run the following to start postfix

`systemctl enable postfix`

4. Restart postfix

`systemctl restart postfix`

5. Check Status of Postfix

`systemctl status postfix`

you will see that it is "active (running)"

## Testing Postfix Server

1. Create a user for testing named "postfixtester"

`useradd postfixtester`

2. Add password for postfixtester user

`passwd postfixtester`

Type in password and retype in password.

3. Check the server access

`telnet localhost smtp`

and it should look like this if it works

```bash
Trying ::1...
Connected to localhost.
Escape character is '^]'.
220 smtp.example.local ESMTP Postfix
```

4. Press "ctrl + ]" and you are in the telnet session.

enter in the following

```bash
ehlo localhost
250-smtp.example.local
250-PIPELINING
250-SIZE 10240000
250-VRFY
250-ETRN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
```
