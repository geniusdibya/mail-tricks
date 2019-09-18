

# Send mails with javascript 
  

In this post will walk you through the steps for sending email using Smtp.js(free js library).
Sending an email directly using client-side javascript without any server configurations with Gmail SMTP.

## Introduction

SMTP is a protocol used to send a specific type of data (mail) to destined server followed by the recepient.
Mostly these type of data sharing requires secured connections and user credentials, thus sending mails irectly through browser is not recommended, but still if you have your own SMTP server, and a robust validator then you may proceed with the approach of sending mails from Javascript (browser).

## Pre requisites

* Your favourite Editor (Sublime, Visual Code, CLI editors, etc)
* Browser (to test the code)
* SMTP Configurations (smtp server details and authentication credentials)

Since we will be using **gmail smtp**, you'll have to make sure that the below settings are turned off.

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

Let’s begin with the code for sending email:

## Lets Begin..

Create below files in a folder,
* index.html  
* index.js  
  
1) Firstly we will use a script file provided by the SMTP js library that helps sending the mail configurations and the mail template to smtpjs.com which will then login to gmail smtp on your behalf to send the mail.  
  
		cdn: <script src="https://smtpjs.com/v3/smtp.js"></script>

>Also we can download the smtp.js from “[https://www.smtpjs.com/](https://www.smtpjs.com/)”

Once we include the script, we will get a global variable in javascript named **Email** (provided by smtp.js)

### Step 1
Include the SMTP library in index.html  
``` html
#index.html
<head>
	<script src="[https://smtpjs.com/v3/smtp.js](https://smtpjs.com/v3/smtp.js)"></script>  
</head>  
  ```

### Step 2
Then include a script file which will have the SMTP details and a mail template. Just add the file name for now, we will create it soon..


``` html
#index.html
<head>
	<script src="https://smtpjs.com/v3/smtp.js"></script>
	<script src="index.js"></script>
</head>
```

### Step 3
In HTML body we will add a button that will simply trigger the mail configured in a javascript function

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

### Step 4
Let’s now write the js function which does the magic of sending mails using **smtpjs.com**
  Create a new file and name it *index.js*, the same file that we have included in the HTML (*index.html*) file.
  
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
  
Above is the basic configurations which sends a post request to the SMTP server and gets back the response  (*success response(OK) or failure response*)
  
  #### All good to go now..
  Open the folder where you have the file index.html and index.js, right click on the index.html file  > open in browser
Once the browser loads the page, click on the buton **Send Email** 

Thats it.. the mail is send successfully...
... (if not, then please validate the google account settings > security >  2 Factor Authentication OR Allow less secure application settings ) *mentioned  above in **pre requisites***

### Step 5
We can also send mails to multiple users the ‘TO’ property which can be an array instead of a single entry.


``` javascript
function sendEmail() {
	Email.send({
	Host: "smtp.gmail.com",
	Username : "sender1 email address, sender2 email address",
	Password : "email password",
	To : 'recipient’s email address',
	From : "sender’s email address",
	Subject : "email subject",
	Body : "email body",
	}).then(
		message => alert("mail sent successfully")
	);
}
```

### Step 6
We can also send the attachments.

  ```javascript
function sendEmail() {
	Email.send({
	Host: "smtp.gmail.com",
	Username : "sender1 email address, sender2 email address",
	Password : "email password",
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

### Some Tips:

The strange thing that you'll notice that there wont be any exceptions (only success) after clicking the send button. 
Thats because your codes smtp connection to gmail is with smtpjs.com, once you click the button then the js code simply sends the content and configurations to smtpjs.com and the actual call to gmail will never be visible to the front end, only a 200 OK status is sent by smtpjs.com.

You would never know whether the mail got sent or no.

So always make sure if you are using any other applications to send mails through gmail smtp, turn 2 factor authentication OFF and turn Allow less secure apps setting ON

## Conclusion

Thats how we acomplished the job of sending mail using Javascript on frontend.
Below is the complete code:
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


### Happy Coding.....
