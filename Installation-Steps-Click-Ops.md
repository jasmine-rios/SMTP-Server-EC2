# Installation-Steps-Click-Ops

## Create EC2 instance

1. In AWS go to EC2 and launch an instance.

2. Choose the AMI as CentOS 7 and use t2.micro as it will be test. Keep the rest default and download key. Create the instance.

3. In the security group for the instance add the following inbound rules:

- SSH, custom, your IP (Find public IP from doing this in terminal: `curl icanhazip.com`)

- SMTP from anywhere

- SMTPS from anywhere

- Port 587 from anywhere (SMTP via TLS)

- Port 2525 from anywhere (Alternate SMTP port)

4. SSH into instance in terminal

`ssh -i <name of key> centos@<public IP address>`

Type yes when prompted to save host.

5. 