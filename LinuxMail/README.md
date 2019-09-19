

# Linux CLI to send mails

We live in an automated world, where numerous events like booking a ride or ordering food online, everything is automated in some or the other way. Similarly notifications these days are way too important to stay updated among these automated activities. Notifications can be emails, sms, push notifications, etc. I'll demonstrate you the simplest way of sending emails.

In this post we will dive in to process of sending mails from terminal or from a shell script from linux operating system, using some cli tools.

## Introduction

Cron reports, script logs, customer registrations, ecommerce purchase orders, bank statements, etc, these events has to notify other connected systems, customers or you might want to send these information over mail to some important peoples. This rises a demand to have a utility that will send mails for you.

Definitely there are other ways, but lets explore the tool thats easy to install and easy to code.

With simple libraries and a little configurations, you can have a cli tool in your linux os that will help you send mails from the terminal.


## Pre Requisites
* Linux operating system
* SMTP Configurations (smtp server details and authentication credentials)
* Your favourite editor (Optional)

yes thats all you need.

Sending mail requires a registered SMTP server that accepts the mail template and recepient details and forwards the mail to the destination.

We will be using **gmail smtp**, you'll have to make sure that the below settings are turned off.

>Configurations to be done before using Google SMTP:

* To use gmail smtp you'll need to allow access for less secure apps. (from google account settings)
> visit here to change the settings: [https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps)


![gaccount_setting](https://i.imgur.com/drZLivS.png)

* Disable 2-step factor authentication.
>link: [https://myaccount.google.com/security](https://myaccount.google.com/security)



#### Google SMTP server configurations would look somethng like this:
- SMTP Server: smtp.gmail.com
-   SMTP Username: gmail-address
-   SMTP password: gmail-password.
-   SMTP Port: 587
-   TLS/SSL: Required.

## Its time to open up a terminal.....
There are different ways and different tools using which you can send mails, we will walk through 2 different tools and different use cases.


## sSMTP

### Step 1

Lets install ssmtp to proceed by following command.

	sudo apt-get install ssmtp

> For CentOS : **sudo yum install ssmtp**
>In CentOS you may see an error during installation as  **"package ssmtp is not available"**,
>Below command is helpful to fix the issue.
>>sudo yum --enablerepo=extras install epel-release

###  Step 2
Once ssmtp gets installed successfully, lets set some global configurations used for mailing.

Open the file ***/etc/ssmtp/ssmtp.conf*** in terminal
	sudo vim /etc/ssmtp/ssmtp.conf

edit the above file with below details
```
mailhub=smtp.gmail.com:587
useSTARTTLS=YES
AuthUser=username-here
AuthPass=password-here
TLS_CA_File=/etc/pki/tls/certs/ca-bundle.crt
```

Above **"mailhub"** is the smtp server address with host:port

Now you are all set to sending mails from CLI
### Step 3

This step can have multipe use cases, lets explore some of the use cases

#### Use case 1
	echo "Test message from Linux server using ssmtp" | sudo ssmtp -vvv revceivers-email@gmail.com

>-vvv is the verbosity to see the logs during sending the mail

Above code will send the mail without any issue if the configurations are set correctly and the pre requisite steps are followed.

#### Use case 2
Lets now try same from a shell script.
Open any editor and create a file and name it mail.sh

```bash
#!/bin/sh  
SUBJECT="Test Subject"
TO="crazy@yopmail.com"
MESSAGE="Hey There! This is a test mail"

echo $MESSAGE | sudo ssmtp -vvv $TO
```


```console
crazy@kali:~/Documents/Scripts$ sudo chmod 755 mail.sh 
crazy@kali:~/Documents/Scripts$
crazy@kali:~/Documents/Scripts$ sudo ./mail.sh 
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

You can see teh error above
**ssmtp: Authorization failed (535 5.7.8 https://support.google.com/mail/?p=BadCredentials u65smyez14952a76922r5pfui.104 - gsmtp)**

>Enable **"Allow less secure app"** in google accounts settings

Or, the provided credentials can be invalid, make sure you have the correct credentials.

Lets Re-run it after enabling the google settings and validating the creds.
```console
crazy@kali:~/Documents/Scripts$ ./mail.sh 
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
crazy@kali:~/Documents/Scripts$ 
```

##  mailx

### Step 1
Lets install mailx in terminal.

> On debian system mail is part of mailutils, and on RedHat mail is part of mailx,
 so in relevant systems use the command below:

***Debian***
	
	sudo apt-get install mailutils
***RedHat***

	sudo yum install mailx

### Step 2

Same as step 2 in above methodology.
Provide the smtp server details and do the corresponding settings in google account settings.

> You may come across error like :
``` console
mail: cannot send message: Process exited with a non-zero status
```
If this error troubles you, then please make sure you have the valid configurations in the **/etc/ssmtp/ssmtp.conf file** and if this file doesnt exists then create it and provide the details

```
mailhub=smtp.gmail.com:587
useSTARTTLS=YES
AuthUser=username-here
AuthPass=password-here
TLS_CA_File=/etc/pki/tls/certs/ca-bundle.crt
```

### Step 3

#### Usage 1
In your terminal type the command to send the mail and see the output.
```console
crazy@kali:~/Documents/Scripts$ echo "My Message Body" | mail -s "My Subject" crazy@yopmail.com --debug-level 6
mail: sendmail binary: /usr/sbin/sendmail
mail: Getting auth info for UID 1000
mail: source=system, name=crazy, passwd=x, uid=1000, gid=1000, gecos=, dir=/home/crazy, shell=/bin/bash, mailbox=/var/mail/crazy, quota=0, change_uid=1
mail: Getting auth info for UID 1000
mail: source=system, name=crazy, passwd=x, uid=1000, gid=1000, gecos=, dir=/home/crazy, shell=/bin/bash, mailbox=/var/mail/crazy, quota=0, change_uid=1
mail: mu_mailer_send_message(): using From: crazy@kali
mail: Sending headers...
mail: Sending body...
mail: /usr/sbin/sendmail exited with: 0
crazy@kali:~/Documents/Scripts$ 
```


#### Usage 2

```console
crazy@kali:~/Documents/Scripts$ sudo mail -s "My subject" crazy@yopmail.com
Cc: crazy1@yopmail.com
Type your message body
new line 
new line
to end this type Ctrl+d
crazy@kali:~/Documents/Scripts$ 

```

#### Usage 3

You can append text contents from a file directly as shown below,
```console
crazy@kali:~/Documents/Blogs$ echo "My Message Body" | mail -s "My Subject" crazy@yopmail.com --debug-level 6 < mail.txt 
mail: sendmail binary: /usr/sbin/sendmail
mail: Getting auth info for UID 1000
mail: source=system, name=crazy, passwd=x, uid=1000, gid=1000, gecos=, dir=/home/crazy, shell=/bin/bash, mailbox=/var/mail/crazy, quota=0, change_uid=1
mail: Getting auth info for UID 1000
mail: source=system, name=crazy, passwd=x, uid=1000, gid=1000, gecos=, dir=/home/crazy, shell=/bin/bash, mailbox=/var/mail/crazy, quota=0, change_uid=1
mail: mu_mailer_send_message(): using From: crazy@kali
mail: Sending headers...
mail: Sending body...
mail: /usr/sbin/sendmail exited with: 0
crazy@kali:~/Documents/Blogs$
```

> In some cases there you can see some issues while using the above command
>Weird Warning:  **mail: Null message body; hope that's ok**

The issue can be because of failure of the echo or the file output, that unfortunately gets passed as empty output as an input body to mail command, so to avoid that, please make sure you have correct content as input to mail

## telnet



Well..
**Cheers!!!!!!!** there goes the mail...

## Conclusion
So we saw 2 different ways to send mail using cli of linux os.

There a lot you could do with these mailing tool like adding multiple recepient, adding people to Cc, etc.
To know more about it just type the mail command and --help to know more about the configs for sending mails.

> Note: If you are using gmail's smtp server
>**"username and password not accepted"**

This is the first error that you might see after the execution of the script, so just make sure you have turned **Allow less secure apps: ON** from your google account settings.
> After enabling this toggle, try re-running the script after some time, google might have some delay in enabling this toggle. and try refreshing the google account setting's page to check if the toggle is enabled.
> This toggle is later disabled by google if not in use.
