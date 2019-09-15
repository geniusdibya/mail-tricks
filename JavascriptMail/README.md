# Send mails with javascript 
  

In this post will walk you through the steps for sending email using Smtp.js(free js library).
Sending an email directly using client-side javascript without any server configurations with Gmail SMTP.


Configurations to be done before using Google SMTP:
* To use gmail smtp you'll need to allow access for less secure apps. (from google account settings)

* Disable 2-step factor authentication.


### Google SMTP server configurations:
- SMTP Server: smtp.gmail.com
-   SMTP Username: gmail-address
-   SMTP password: gmail-password.
-   SMTP Port: 587
-   TLS/SSL: Required.

Let’s begin with the code for sending email:

We will be using an index.html and index.js file.  
|-Root  
|- index.html  
|- index.js  
  
1) Firstly we will use the CDN or script file provided by the SMTP js library:  
  
		cdn: <script src="https://smtpjs.com/v3/smtp.js"></script>

>Also we can download the smtp.js from “[https://www.smtpjs.com/](https://www.smtpjs.com/)”

Include the SMTP library in index.html  
``` html
<head>
	<script src="[https://smtpjs.com/v3/smtp.js](https://smtpjs.com/v3/smtp.js)"></script>  
</head>  
  ```

2) Then include our custom script file which contains the js code to send email.

index.html  
``` html
<head>
	<script src="https://smtpjs.com/v3/smtp.js"></script>
	<script src="index.js"></script>
</head>
```

3) We will create a button tag to trigger the mail

index.html  
``` html
<body>  
<form method="post">
<input type="button" value="Send Email" onclick="sendEmail()"/>

</form>  
</body>
```
4) Let’s now write the js function which sends POST request to the **smtpjs.com**

index.js  
  
/**Basic structure for email:*/  
```javascript
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
  

5) We can also send mails to multiple users the ‘TO’ property which can be an array instead of a single entry.

/** Multiple recipient’s */
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

6) We can also send the attachments.

/** send attachment */  
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


>Note :  Sending mails directly from frontend is not recommended, since it is visible to client, an attacker may use it for some suspicious activity.
>Alwasy validate the mailing request on backend to reduce the risk of security issues.