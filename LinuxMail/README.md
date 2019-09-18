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

## Step 1

#### Installation
Lets install SMTP server to proceed with mailing.

	sudo apt-get install ssmtp

>if you are using centos, then you may see an error during installation as  **"package ssmtp is not available"**,
>Below command is helpful to fix the issue.
>>yum --enablerepo=extras install epel-release
## Step 2
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

	echo "Test message from Linux server using ssmtp" | sudo ssmtp -vvv revceivers-email@gmail.com

>-vvv is the verbosity to see the logs during sending the mail



There are also some other tools to send mail from CLI, like **mailx**

	sudo mail -s "Enter the subject" revceivers-email@gmail.com

Hit the above command, add **Cc**, followed by the **mail body**, then hit **Ctrl+d** to send the mail.


**Cheers!!!!!!!** there goes the mail...

> Note: If you are using gmail's smtp server
>**"username and password not accepted"**

This is the first error that you might see after the execution of the script, so just make sure you have turned **Allow less secure apps: ON** from your google account settings.
> After enabling this toggle, try re-running the script after some time, google might have some delay in enabling this toggle. and try refreshing the google account setting's page to check if the toggle is enabled.
> This toggle is later disabled by google if not in use.
