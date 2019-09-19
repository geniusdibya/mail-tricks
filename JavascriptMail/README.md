# Send mails with javascript 

## Introduction

In this tutorial you will learn the steps for sending email using Smtp.js (a free JS library). Using this you will be able to directly send email using client-side javascript without any server level configurations.

SMTP is a protocol used to send a specific type of data (i.e. email) to destined server followed by the recipient. Mostly these type of data sharing requires secured connections and user credentials, thus sending mails directly through browser is not recommended, but still if you have your own SMTP server, and a robust validator, then you may proceed with the approach of sending mails from Javascript (browser).

## Prerequisites

* Your favourite Editor (Sublime, Visual Code, CLI editors, etc)
* Browser (to test the code)
* SMTP Configurations (smtp server details and authentication credentials)
* Configure Gmail SMTP/ready with any other custom SMTP server details. In this tutorial, you will be using **Gmail SMTP** to send emails, so make sure that;
 1.  **Allowed access in Gmail for less secure apps:** To use Gmail SMTP you'll need to allow access for less secure apps from google account [settings](https://myaccount.google.com/lesssecureapps "settings"). Turning the below settings off will allow your Javascript to connect to Gmail.
![gaccount_setting](https://i.imgur.com/drZLivS.png)

 2. **Disabled 2-step factor authentication:** As you're going to connect to gmail remotely using a program, so 2-step factor authentication should be disabled. Click [here](https://myaccount.google.com/security "here") to learn more on how to disable 2FA.

**Google SMTP server configurations would look something like this:**
-   SMTP Server/Hostname: smtp.gmail.com
-   SMTP Username: [Your Gmail Address]
-   SMTP password: [Your Gmail Password]
-   SMTP Port: 587
-   TLS/SSL: Required

Let’s begin with the code for sending email:

## Writing JavaScript and integrating with Gmail to send mail

### Step 1
Create below two files in a folder:

```index.html```
```index.js```

### Step 2

Download the [smtp.js](https://smtpjs.com/v3/smtp.js "smtp.js") or include a script tag to https://smtpjs.com/v3/smtp.js

Here, in this tutorial you will be using this open source SMTPJS to send emails, so that you really don't have to do much of server level configurations.
SMTP.js library will help connecting with Gmail SMTP on your behalf to send the mail.

Once you include the script, you will get a global variable in javascript named **Email** (provided by smtp.js)

Open the `index.html` and include the SMTPJS library like shown below:

``` html
#index.html
<head>
	<script src="[https://smtpjs.com/v3/smtp.js](https://smtpjs.com/v3/smtp.js)"></script>  
</head>
  ```

### Step 3
Next step is to include a script file which will have the SMTP details and a mail template. Just add the file name `index.js` for now, in the following steps you will learn what all code to be added to this JS file.

```html
#index.html
<head>
	<script src="https://smtpjs.com/v3/smtp.js"></script>
	<script src="index.js"></script>
</head>
```

### Step 4

Add a button in the HTML body, this will simply trigger the mail configured in a javascript function:

index.html  
``` html
#index.html
...
<body>  
	<form method="post">
		<input type="button" value="Send Email" onclick="sendEmail()"/>
	</form>  
</body>
```

### Step 5
Now write the JS function which will do the magic of sending mails using **smtpjs.com**. Create a new file and name it *index.js*, the same file that you have included in the HTML (*index.html*) file.
  
```javascript
//index.js  
function sendEmail() {
	Email.send({
	Host: "smtp.gmail.com",
	Username : "<sender’s email address>",
	Password : "<email password>",
	To : '<recipient’s email address>',
	From : "<sender’s email address>",
	Subject : "<email subject>",
	Body : "<email body>",
	}).then(
		message => alert("mail sent successfully")
	);
}
```
  
Above is the basic configurations which sends a post request to the SMTP server and gets back the response:  
```success response(OK) or failure response```

All good to go now.
Open the folder where you have the file index.html and index.js, right click on the index.html file  > open in browser
Once the browser loads the page, click on the buton **Send Email**.

That's it.. the mail is send successfully.

If the mail is not sent from your Javascript, then please validate the Google account settings > security >  2 Factor Authentication OR Allow less secure application settings as mentioned  above in the prerequisites.


### Step 6 (Optional)

You can also send mails to multiple users. For that the you need to use the ‘TO’ property which can be an array instead of a single entry.
Here's an sample 

``` javascript
function sendEmail() {
	Email.send({
	Host: "smtp.gmail.com",
	Username : "Your Gmail Address",
	Password : "Your Gmail Password",
	To : 'recipient_1_email_address, recipient_2_email_address',
	From : "sender’s email address",
	Subject : "email subject",
	Body : "email body",
	}).then(
		message => alert("mail sent successfully")
	);
}
```

### Step 7 (Optional)

You can also send emails with attachments. Use the following code for sending attachments in email using Javascript:

  ```javascript
function sendEmail() {
	Email.send({
	Host: "smtp.gmail.com",
	Username : "Your Gmail Address",
	Password : "Your Gmail Password",
	To : 'recipient’s email address',
	From : "sender’s email address",
	Subject : "email subject",
	Body : "email body",
	Attachments : [
		{
			name : "smtpjs.png",
			path:"https://networkprogramming.files.wordpress.com/2017/11/smtpjs.png"
		}]
	}).then(
		message => alert("mail sent successfully")
	);
}
```

### Note

The strange thing that you'll notice that there won't be any exceptions (you will mostly get success) after clicking the send button. That's because your code js code simply sends the content and configurations to smtpjs.com and the actual call to Gmail will never be visible to the front end, only a 200 OK status is sent by smtpjs.com. Connection to Gmail actually happens on the smtpjs end. You would never know whether the mail got sent or no.

So always make sure if you are using any other applications to send mails through Gmail SMTP, turn 2 factor authentication OFF and turn Allow less secure apps setting ON.

Below is the complete working code for your reference:

```html
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<script src="https://smtpjs.com/v3/smtp.js"></script>  
	<script type="text/javascript">
		function sendEmail() {
			Email.send({
				Host: "smtp.gmail.com",
				Username : "<sender’s email address>",
				Password : "<email password>",
				To : '<recipient’s email address>',
				From : "<sender’s email address>",
				Subject : "<email subject>",
				Body : "<email body>",
			})
			.then(function(message){
				alert("mail sent successfully")
			});
		}
	</script>
</head>
<body>  
	<form method="post">
		<input type="button" value="Send Email" onclick="sendEmail()"/>
	</form>  
</body>
</html>
```

## Conclusion

Hope, the tutorial helped and you are now able to send emails using Javascript from the front end.
**Happy Coding...**
