
# Sending mails in Ruby with SMTP

Ruby comes with an out of the box library named "Net::SMTP", which helps you to send internet mails using Simple Mail Transfer Protocaol.

>Note: This library just helps you to send the Mail to an SMTP server via network with or without TLS.
>We are using version -  **ruby 2.5.5p157**

Here we will be configuring gmail's smtp to send mails, and this configurations are mostly similar to other smtp connections.

To begin with the steps we will first import the library as below,
```ruby
require 'net/smtp'
```
now lets have some variables declared for some configurations.
```ruby
FROM_EMAIL = "my-email-id@gmail.com"
PASSWORD = "my-email-password"
TO_EMAIL = "receivers-email@gmail.com"
```
Create an instance of smtp client
```ruby
smtp = Net::SMTP.new 'smtp.gmail.com', 587
smtp.enable_starttls
```
Above we have created an instance of smtp client for host **smtp.gmail.com** on port 587,
and then we enable tls for securing the connections.


Then proceed with composing of the mail..
```ruby
message = <<END_OF_MESSAGE
From: SomeName <my-email-id@gmail.com>
To: ToName <receivers-email@gmail.com>
Subject: Mail From Ruby 
 
Hello there!!
This is a message from Ruby.
END_OF_MESSAGE
```
now the last step, start the smtp client, send the mail and then close the connection,
``` ruby
smtp.start('received-from-goes-here', FROM_EMAIL, PASSWORD, :plain)
smtp.send_message(message, FROM_EMAIL, TO_EMAIL)
smtp.finish()
```
You've just sent your first mail using Ruby..

>**Tip**: You can also files in the template as an attachments, with base64 encoded and adding the required headers for MIME and content type.

**Thats all folks!!**

Somewhere during execution...........
>**"username and password not accepted"**

This is the first error that you might see after the execution of the script, so just make sure you have turned **Allow less secure apps: ON** from your google account settings.
> After enabling this toggle, try re-running the script after some time, google might have some delay in enabling this toggle. and try refreshing the google account setting's page to check if the toggle is enabled.
> This toggle is later disabled by google if not in use.