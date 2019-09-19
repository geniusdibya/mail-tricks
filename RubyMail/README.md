# Sending mails in Ruby with SMTP

## Introduction
Ruby is a popular scripting programming language, uses Object oriented programming fundamentals, light weight and much more like a spoken language. It helps you to complete various programming operations in few lines of code, and that's what make it more elegant and powerful.

In this post you will learn how to send emails using ruby and what are some of the most common errors you might surface while sending mails via SMTP.

tl;dr

## Prerequisites to Send Emails with Ruby on Rails

We will be sending mails using core library of Ruby, so there is no major dependencies. Just ensure you have the following:

* **Ruby**
Note: To support any dependencies, ruby has a package manager named [RubyGems](https://rubygems.org/).
A package manager is a utility that helps you to store and share the code that you have written, and also you can use the available libraries via RubyGems.

* **Your favourite editor** (Sublime, Visual Code, CLI editors, etc)
* **Environment setup** *(Skip, if you already have a Ruby environment)*
In case you have not setup Ruby. then follow the below steps to setup the Ruby environment:
##### Steps to install Ruby on Windows
Below is the link to download binaries and executable files:
>[https://www.ruby-lang.org/en/downloads/](https://www.ruby-lang.org/en/downloads/)

 You may notice there are two downloadable files viz- **Ruby+Devkit 2.6.X** and **Ruby 2.6.X** without devkit.

 Visit below link to learn more about dev kit:
>[https://github.com/oneclick/rubyinstaller/wiki/Development-Kit](https://github.com/oneclick/rubyinstaller/wiki/Development-Kit)

 ##### Error Handling
During the installation, you might surface following errors too:

 **Error 1:** **Warning in Windows 10**

 Windows protected your PC
 **Windows Defender SmartScreen prevented an unrecognized app from starting. Running this app might put your PC at risk.**
> Solution: If you face this issue, then simply click "more info" and then click on "run anyway".

 Next, make sure you select "Add Ruby executable to your PATH" *(Keep the default options selected as it is)*.

 Once the setup is completed, open command prompt and run 
```command
ruby -v
```

 Here's output from the above command:
```
ruby 2.6.4p104 (2019-08-28 revision 67798) [x64-mingw32]
```
If your output is something in lines with the above, this means you are all set with ruby environment and its time to proceed further writing the code to send email.

 If you are using a operating system other than windows then please follow the helpful link below for the Ruby installation:
>[https://www.ruby-lang.org/en/documentation/installation/](https://www.ruby-lang.org/en/documentation/installation/)


## Writing the code to Send Email using Ruby

Sending mail requires you to generate the raw email copy which is mostly referred as an eml file. This raw format of email consists of email header and content as per the RFC standards.

Once you are ready with this raw copy of email, this packet need to be send to some SMTP server which have the right protocols to deliver the email. Here, in this tutorial Gmail SMTP has been used to send email. But, you can use other third party email service providers too (e.g. Pepipost). The steps will be mostly similar.

Ruby comes with an out of the box library named "Net::SMTP", which helps you generate this raw copy of the email and send the same using any SMTP of your choice.

>Note: Net::SMTP library helps you to send the mail to an SMTP server via network with or without TLS.

### Step 1
Create a new file named *mail_script.rb* and open it in your favourite editor.:
```ruby
mail_script.rb
```

### Step 2
Let's write the code to send email. To begin with the real function to send email, you need to first import few of the important libraries as show below:

```ruby
require 'net/smtp'
```
Declare the key email sending parameters:

```ruby
FROM_EMAIL = "my-email-id@gmail.com"
PASSWORD = "my-email-password"
TO_EMAIL = "receivers-email@gmail.com"
```

Create an instance of SMTP client:

```ruby
smtp = Net::SMTP.new 'smtp.gmail.com', 587
```

In the above code, you have actually created an instance of SMTP client for host **smtp.gmail.com** to connect on port 587.
Using port 587 will help making a secured connection to SMTP server.


Next, proceed with composing of the mail.

```ruby
message = <<END_OF_MESSAGE
From: SomeName <my-email-id@gmail.com>
To: ToName <receivers-email@gmail.com>
Subject: Mail From Ruby 
 
Hello there!!
This is a message from Ruby.
END_OF_MESSAGE
```
The last step is to initiate the SMTP client, send the mail and then close the connection. Here is the code snipped for the same:

``` ruby
smtp.start('received-from-goes-here', FROM_EMAIL, PASSWORD, :plain)
smtp.send_message(message, FROM_EMAIL, TO_EMAIL)
smtp.finish()
```

### Step 3
Open command prompt and navigate to the folder that contains the file mail_script.rb and run the command below to execute the script

``` ruby
ruby mail_script.rb
```

Hope you will get the success message and email will be sent to the recipient/to email address defined in your program.

If you getting some strange errors, then you can continue reading the next half of this tutorial, which explains about some of the most common error and exceptions. In case the error, which you faced is not listed in this doc, then please feel free to contribute below in the comments.

**Error 1**

```
530 5.7.0 Must issue a STARTTLS command first .. gsmtp (Net::SMTPAuthenticationError)
```

This means that your connection is not secured, to make it secure you must enable TLS or SSL.

### Step 4
So lets add one more line to the code:

``` ruby
smtp.enable_starttls
```
Add this line immediately after creating an SMTP clients instance.

Now, if you try to run the script, you'll probably see the next error (only if you are using Gmail's SMTP server to send mails)


**Error 2**

```
Error: username and password not accepted
```

This is the error that you might see after the execution of the script, so just make sure you have turned **Allow less secure apps: ON** from your google account settings.

> Solution: After enabling this toggle, try re-running the script after some time, google might have some delay in enabling this toggle. and try refreshing the google account setting's page to check if the toggle is enabled.
> This toggle is later disabled by google if not in use.
> visit here to change the settings: [https://myaccount.google.com/lesssecureapps](https://myaccount.google.com/lesssecureapps)

![gaccount_setting](https://i.imgur.com/drZLivS.png)

### Note
In some cases you can see the exception below:
```
check_auth_response 534-5.7.9 Application-specific password required. Learn more at (Net::SMTPAuthenticationError)
```
This is again because of google security, *If you have enabled 2-factor authentication security then this error can be resolved by disabling the feature from google accounts*
> link: [https://myaccount.google.com/security](https://myaccount.google.com/security)


Here's the complete running code for your reference:
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


## Conclusion
Hope the steps explained above were useful and you were able to successfully sent your first mail.
