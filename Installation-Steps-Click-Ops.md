# Installation-Steps-Click-Ops

## Obtain a domain

You can obtain a domain to try this from domain sellers like Godaddy, AWS Route 53, and others.

I have a domain as I have for a personal project
## Create EC2 instance

1. In AWS go to EC2 and launch an instance.

2. Choose the AMI as CentOS 7 and use t2.micro as it will be test. Keep the rest default and download key. Create the instance.

3. In the security group for the instance add the following inbound rules:

- SSH, custom, your IP (Find public IP from doing this in terminal: `curl icanhazip.com`)

- SMTP from anywhere

- SMTPS from anywhere

- Port 587 from anywhere (SMTP via TLS)

- Port 2525 from anywhere (Alternate SMTP port)

## SSH into EC2 Instance

1. SSH into instance in terminal

`ssh -i <name of key> centos@<public IP address>`

Type yes when prompted to save host.

2. Now that you are in the centos instance do the following. (HINT: You could have also done this in the user data before launching instance if you are going to create multiple instances)

```bash
# Enter Sudo

sudo -i

# Install postfix and vim

yum install postfix

yum install vim

# edit the postfix main.cf file 
# Remember that i let's you change the file
vim /etc/postfix/main.cf
# Uncomment the following and add these changes
myhostname = smtp.example.local

mydomain = <insert your domain name>
# Uncomment the following 
myorigin = $mydomain

# Make sure it's uncommened and set ipv4

inet_interfaces = <public ip4 from instance>

# Make sure it's uncommented

inet_protocols = all

# Make sure it's uncommented

my destionation = $myhostname, localhost.$mydomain, localhost

# Uncomment and add IP range for Public and Private IPs from AWS

mynetworks = <public IP/CIDR> <Private IP/CIDR>

... More to come 

# To save your work use escape key then type :x



```