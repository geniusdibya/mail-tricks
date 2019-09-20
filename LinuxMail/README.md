# Linux CLI to send mails

Notifications are getting more important these days to stay updated among various online activities. Moreover, these notifications can be emails, SMS or push. This tutorial is going to demonstrate you the simplest way of sending notifications, over one of the most popular channel that is email. You are going to learn the process of sending emails from the terminal or a shell script from a Linux operating system, using some of the popular CLI tools.

## Introduction

This tutorial is going to help you in sending critical server-level emails like Cron reports, script logs, customer registrations, receipt, bank statement over email. There are many ways to send emails from the server, but let's explore the tool that's easy to install and easy to code.

With simple libraries and little configurations, you can have a CLI tool in your Linux OS that you can use to send mails from the terminal.

## Prerequisites
* Linux operating system
* SMTP Configurations (SMTP server details and authentication credentials)
* Your favourite editor (Optional)
* Configure Gmail SMTP/ready with any other custom SMTP server details. In this tutorial, you are going to use **Gmail SMTP** to send emails, so make sure that;
 1.  **Allowed access in Gmail for less secure apps:** To use Gmail SMTP you'll need to allow access for less secure apps from google account [settings](https://myaccount.google.com/lesssecureapps "settings"). Turning the below settings off is going to allow your Javascript to connect to Gmail.
![gaccount_setting](https://i.imgur.com/drZLivS.png)

 2. **Disabled 2-step factor authentication:** As you're going to connect to Gmail remotely using a program, so 2-step factor authentication should be disabled. Click [here](https://myaccount.google.com/security "here") to learn more on how to disable 2FA.

**Google SMTP server configurations would look something like this:**
-   SMTP Server/Hostname: smtp.gmail.com
-   SMTP Username: [Your Gmail Address]
-   SMTP password: [Your Gmail Password]
-   SMTP Port: 587
-   TLS/SSL: Required

## Its time to now open terminal
There are various tools and libraries which you can install to send emails from the terminal. Few of the popular libraries are:
- sSMTP
- Mailx

In this tutorial, you're going to learn the steps on how to install and use sSMTP to send mails from your Linux command line. Click here, in case you want to learn how to install and use Mailx to send mails from your Linux command line.

## How to install sSMTP to send mails from your Linux command line (CLI)

### Step 1

Use the below command to install ssmtp:

	sudo apt-get install ssmtp

**Optional:**
CentOS users can use the below command to install ssmtp:

	sudo yum install ssmtp

In CentOS, you may see an error during installation as  **"package ssmtp is not available"**, in such a case below command, is going to be helpful to fix the issue:

	sudo yum --enablerepo=extras install epel-release

###  Step 2
Once ssmtp installed successfully, you need to make the below global configurations which required for sending mail.

Open the following file in your favourite editor:

	sudo vim /etc/ssmtp/ssmtp.conf

Edit the above file with the below details:

```
mailhub=smtp.gmail.com:587
useSTARTTLS=YES
AuthUser=username-here
AuthPass=password-here
TLS_CA_File=/etc/pki/tls/certs/ca-bundle.crt
```
The above configuration is going to be used to send email using your Gmail SMTP. In case you want to use some other third-party SMTP, then mention the hostname of the same. e.g. if you want to use Pepipost SMTP, then instead of smtp.gmail.com, you need to mention smtp.pepipost.com in the mailhub parameter. **mailhub** is used for SMTP server address which consists of two part host:port

Now you are all set to sending mails from the command line (CLI).

### Step 3

There are multiple ways to use ssmtp command to send emails.

#### Case 1: Send mail directly from the command line

For this, copy-paste the below command, and you're ready to send email from your command line:

	echo "Test message from Linux server using ssmtp" | sudo ssmtp -vvv revceivers-email@gmail.com

>-vvv is the verbosity to see the logs while sending the mail

#### Case 2: Send mail from a shell script

You can use the same ssmtp to send mail from a shell script too. For that, open your preferred editor and create a shell script file with name say `mail.sh` and copy-paste the below code:

```bash
#!/bin/sh  
SUBJECT="Test Subject"
TO="crazy@yopmail.com"
MESSAGE="Hey There! This is a test mail"

echo $MESSAGE | sudo ssmtp -vvv $TO
```
Make sure you have set the right permission access to your script file. If not, here is the command to set the permission:

```console
$ sudo chmod 755 mail.sh 
```

Now, the code is ready to be executed. Just run the shell script using the below command:
```console
$ sudo ./mail.sh
```
Hope now you're able to send mails using the shell script too.

Below are few errors/exceptions which you may encounter while sending the mail using ssmtp:

### Error 1
In case while sending the email, if might get the below error as output:
ssmtp: Authorization failed (535 5.7.8 https://support.google.com/mail/?p=BadCredentials u65smyez14952a76922r5pfui.104 - gsmtp)

```console
[<-] 220 smtp.gmail.com ESMTP u65sm14952769pfu.104 - gsmtp
[->] EHLO kali
[<-] 250 SMTPUTF8
[->] STARTTLS
[<-] 220 2.0.0 Ready to start TLS
[->] EHLO kali
[<-] 250 SMTPUTF8
[->] AUTH LOGIN
[<-] 334 VXNlcm5edhbAWU6
[->] dmlzaGFsY2hhasd2dWhhbjIyMTJAZ21haWwuY29t
[<-] 334 EUEeGFzc3dvfcaqQ6
[<-] 535 5.7.8  https://support.google.com/mail/?p=BadCredentials u65smyez14952a76922r5pfui.104 - gsmtp
ssmtp: Authorization failed (535 5.7.8  https://support.google.com/mail/?p=BadCredentials u65smyez14952a76922r5pfui.104 - gsmtp)
crazy@kali:~/Documents/Scripts$ 
```
**Solution:** In such a case, try doing following as solution:
1. Enable **"Allow less secure app"** in your google accounts settings, as explained in the above prerequisites section.

2. The provided login credentials can be invalid. Make sure you have the correct credentials.

Once the issue is fixed, re-run the shell script and the success output will be something like this; 
```console
$ sudo ./mail.sh
```
Output:
```console
[<-] 220 smtp.gmail.com ESMTP h8sm1096aae22880pfo.64 - gsmtp
[->] EHLO kali
[<-] 250 SMTPUTF8
[->] STARTTLS
[<-] 220 2.0.0 Ready to start TLS
[->] EHLO kali
[<-] 250 SMTPUTF8
[->] AUTH LOGIN
[<-] 334 VXNlcm5edhbAWU6
[->] dmlzaGFsY2hhasd2dWhhbjIyMTJAZ21haWwuY29t
[<-] 334 UGFzqc3dmvcmQ36
[<-] 235 2.7.0 Accepted
[->] MAIL FROM:<root@kali>
[<-] 250 2.1.0 OK h8smqer10962480pfo.64 - gsmtp
[->] RCPT TO:<crazy@yopmail.com>
[<-] 250 2.1.5 OK h8smqer10962480pfo.64 - gsmtp
[->] DATA
[<-] 354  Go ahead h8sm10962880pfo.64 - gsmtp
[->] Received: by kali (sSMTP sendmail emulation); Thu, 19 Sep 2019 21:45:14 +0530
[->] From: "root" <root@kali>
[->] Date: Thu, 19 Sep 2019 21:45:14 +0530
[->] Hey There! This is a test mail
[->] 
[->] .
[<-] 250 2.0.0 OK  1568909725 h8smqer10962480pfo.64 - gsmtp
[->] QUIT
[<-] 221 2.0.0 closing connection h8smqer10962480pfo.64 - gsmtp
```
## Conclusion
Hope the steps explained above were useful and you were able to successfully send mail using linux command line (CLI). Feel free to contribute, in case you encountered some issue which is not listed as a part of this tutorials.
Use below comments section to ask/share any feedback.

**<!-- Happy Coding />**

