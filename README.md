# README.md

## Installation-Steps-Click-Ops

Use this to install a test SMTP server in the AWS console. Information on SMTP ports to use was found at (Cloudflare SMTP Breakdown)[https://www.cloudflare.com/learning/email-security/what-is-smtp/]. 

Ideally for real SMTP don't have these SMTP ports open anywhere but narrow it down with the Networks that it's going to send it with.

The steps to edit the /etc/postfix/main.cf to make Postfix work was based off of (Install Postfix on Centos)[https://netcorecloud.com/tutorials/install-centos-postfix/]

For `vim /etc/postfix/main.cf` you need to make sure it is run by root using`sudo -i` or doing `sudo vim /etc/postfix/main.cf`.
If not not using root then the file will be read-only and you will have to exit without saving (:q) then go into sudo.


**PROBLEM 1**

When restarting systemctl there is an error given below:
```bash
# systemctl restart postfix
Job for postfix.service failed because the control process exited with error code. See "systemctl status postfix.service" and "journalctl -xe" for details.
```

Running `systemctl status postfix.service` there is a fatal error
```bash
postfix[1839]: fatal: parameter inet_interfaces: no local interface found for <IP OMITTED> 
```

This is because line 116 in /etc/postfix/main.cf is the public IP of the instance. 
Maybe need to add another IP or can set to all. 

**FIX 1**
For inet_interface you need to use either all, loopback, or specified IP address. 

If wanting the SMTP server to serve mail servers on the internet use `inet_interfaces = all` 

If wanting to keep SMTP server to serve local traffic, will have to be one same private network then you will use

`inet_interface = <IP address>`

You can get the IP address using `ip addr`. 

**PROBLEM 2**
When checking server access you need to this command according to (Install Postfix on Centos)
`telnet localhost smtp`

When running the command you get 
```bash
#telnet localhost smtp
-bash: telnet: command not found
```

**FIX 2**
In order to do telnet you need to have it installed so I added `yum install telnet`

**PROBLEM 3**
When running `telent localhost smtp`
the output is as follows

```bash
# telnet localhost smtp
Trying ::1...
telnet: connect to address ::1: Connection refused
Trying 127.0.0.1...
telnet: connect to address 127.0.0.1: Connection refused
```

**FIX 3**
inet_interface=all needs to be set to allow `telnet localhost smtp` to work.

## Automation Folder

### SMTP-Test-Install-Terraform

### SMTP-Test-Install-Cloudformation

### SMTP-Test_Install-Ansible

