## Cross Site Scripting
What is XSS?
	**Cross Site Scripting (XSS)** is a common attack vector that injects malicious code into a vulnerable web application

**XSS** differs from other web attack vectors (i.e., **SQL Injections**), in that it does not directly target the application itself.
	Instead, the users of the web application are the ones at risk.

A successful cross-site scripting attack can have devastating consequences for an online business's reputation and its relationship with its clients.
![](Pasted%20image%2020240311145133.png)

## Types of XSS
There are **three** main different types of Cross-Site Scripting Vulnerabilities:
- **Reflected XSS**
	- A reflected XSS vulnerability happens when the user input from a URL or POST data is reflected on the page without being stored
- **Persistent or Stored XSS**
	- Stored Cross-Site Scripting vulnerabilities happens when the payload is saved, for example, in a database and then is executed when a user opens the page.
		- Stored cross-site scripting is very dangerous for many reasons
- **DOM-based XSS**
	- The DOM-based XSS vulnerability happens in the DOM (Document Object Model) instead of part of the HTML
These are not mutually exclusive, either. You can have Stored, Reflected DOM-based XSS.

You can also have Stored, Reflected Non-DOM-based XSS too.

**New Types of XSS (easier to understand)**
1. Server XSS
2. Client XSS


## Client XSS
![](Pasted%20image%2020240311145811.png)
```HTML
https://insecure-website.com/status?message=All+is+well. 
<p>Status: All is well.</p>
```
While the web server does not do anything for protection for XSS, the attacked can create the following attack link and email it to the victim:
```HTML
https://insecure-website.com/status?message=<script>/*+Bad+stuff+here...+*/</script>
<p>Status: <script>/* Bad stuff here... */</script></p>
```
## Server XSS
![](Pasted%20image%2020240311145820.png)
A message board application lets users submit messages, which can get displayed to other users:
```HTML
<p>Hello, this is my message!</p>
```
The application doesn't perform any other processing of the data, so an attacker can easily send a message that attacks other users:
```HTML
<p><script>/* Bad stuff here... */</script></p>
```

## DOM XSS


## Impact of XSS


## Ways to Identify and Verify XSS Vulnerabilities


## Preventing Cross-Site Scripting
***PREVENTION????***

•Never trust user input
•Never trust user input
•Never trust user input
•Never trust user input
•Never trust user input
•Never trust user input
•Never trust user input
•Never trust user input
•Never trust user input

## Preventing XSS - Encoding



## Preventing XSS - Validating


## FAQ
