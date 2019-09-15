# Linux CLI to send mails

Cron reports, script logs, etc are very helpful to keep track of things in production or dev environments, you might want to send these information over mail to some important peoples. With a little configurations and library installation you can add a cli tool in your linux os for sending mails from the terminal.

Lets install SMTP server to proceed with mailing.

	sudo apt-get install ssmtp

Lets set some global configurations

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