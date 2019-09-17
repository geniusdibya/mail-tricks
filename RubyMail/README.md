# Sending mails in Ruby with SMTP


In this post we will explore the paradigm fo ruby for sending mails and also try to solve some of the common errors that developers come accross while sending mails via SMTP.

## Introduction 
Ruby is a popular scripting programming language, uses OOP fundamentals, light weight and much more like a spoken language.
It helps you to complete various programming operations in few lines of code, and that's what make it more elegant and powerful.

Ruby is supported by various platforms like Windows, Linux and MacOS.

To support dependencies, ruby has a pakage manager named RubyGems
> [https://rubygems.org](https://rubygems.org/)

A package manager is a utility that helps you to store and share the code that you have written, and also you can use the available libraries via RubyGems.

tl;dr

## Pre Requisites

We will be sending mails using core library of Ruby, so no other dependencies are requrired except following,

* Ruby
* Your favourite Editor


## Environment Setup
The scope of this post is to send emails from a script written in Ruby. This block of code then can be used in any application for sending mails.

We will walk through the libraries involved and steps to send mails.

### Installation (Windows)
Lets install ruby first

Below is the link to download binaries and executable files:
>[https://www.ruby-lang.org/en/downloads/](https://www.ruby-lang.org/en/downloads/)

I'll be installing on Windows,
You may notice there are two downloadable files viz- **Ruby+Devkit 2.6.X** and **Ruby 2.6.X** without devkit.
>visit here to learn more about dev kit: [https://github.com/oneclick/rubyinstaller/wiki/Development-Kit](https://github.com/oneclick/rubyinstaller/wiki/Development-Kit)

First warning in windows 10:
Windows protected your PC
**Windows Defender SmartScreen prevented an unrecognized app from starting. Running this app might put your PC at risk.**
> tip: Click "more info" and then click on "run anyway"

Next, Make sure you select "Add Ruby executables to your PATH" (let the default options be selected as is).

Once the setup is completed, open command prompt and run "ruby -v"
>*ruby 2.6.4p104 (2019-08-28 revision 67798) [x64-mingw32]*

>For other operating system installation steps please visit below link: 
>[https://www.ruby-lang.org/en/documentation/installation/](https://www.ruby-lang.org/en/documentation/installation/)

you'll see something like the above output.
Now you are all set with ruby environment, lets proceed with some coding...

### Its Time To Code Now...
Sending mail would require some connection to the SMTP server to send mail template and headers from the script, so we will use a networking library to achieve connection.

Ruby comes with an out of the box library named "Net::SMTP", which helps you to send internet mails using Simple Mail Transfer Protocaol.

>Note: This library just helps you to send the Mail to an SMTP server via network with or without TLS.

Here we will be configuring gmail's smtp to send mails, and this configurations are mostly similar to other smtp connections.

Create a new file named *mail_script.rb* and open it in your favourite editor
>**mail_script.rb**


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

run the code now

	>ruby mail_script.rb

### Exceptions
*...Something strange happens..... **EXCEPTION!!***

	530 5.7.0 Must issue a STARTTLS command first .. gsmtp (Net::SMTPAuthenticationError)

This means that your connection is not secured, to make it secure you must enable TLS or SSL.

So lets add one more line to the code 
``` ruby
smtp.enable_starttls
```
Add this line immediately after creating an smtp clients instacne.

now if you try to run the script, you'll probably see the next error (only if you are using gmail's smtp serve to send mails)

*you might see this below exception*

	Error: username and password not accepted

This is the error that you might see after the execution of the script, so just make sure you have turned **Allow less secure apps: ON** from your google account settings.
> After enabling this toggle, try re-running the script after some time, google might have some delay in enabling this toggle. and try refreshing the google account setting's page to check if the toggle is enabled.
> This toggle is later disabled by google if not in use.
> visit here to change the settings: [https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps)


![gaccount_setting](https://i.imgur.com/drZLivS.png)


In some cases you can see the exception below:

	check_auth_response 534-5.7.9 Application-specific password required. Learn more at (Net::SMTPAuthenticationError)
This is again because of google security, *If you have enabled 2-factor authentication security then this error can be resolved by diasabling the feature from google accounts*
> link: [https://myaccount.google.com/security](https://myaccount.google.com/security)

## Conclusion
Using Ruby you have successfully sent your first mail..

Heres the complete running code,
```ruby
require 'net/smtp'
FROM_EMAIL = "my-email-id@gmail.com"
PASSWORD = "my-email-password"
TO_EMAIL = "receivers-email@gmail.com"

smtp = Net::SMTP.new 'smtp.gmail.com', 587
smtp.enable_starttls

message = <<END_OF_MESSAGE
From: SomeName <my-email-id@gmail.com>
To: ToName <receivers-email@gmail.com>
Subject: Mail From Ruby 
 
Hello there!!
This is a message from Ruby.
END_OF_MESSAGE

smtp.start('received-from-goes-here', FROM_EMAIL, PASSWORD, :plain)
smtp.send_message(message, FROM_EMAIL, TO_EMAIL)
smtp.finish()
```
**Thats all folks!!**
..
