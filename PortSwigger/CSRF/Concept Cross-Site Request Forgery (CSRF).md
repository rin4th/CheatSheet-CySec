### What is CSRF
Cross-Site Request Forgery is a web security vulnerability that allow an attacker to induce the behavior of victim to perform action that they don't intend to perform. The impact of the attack causes the victim user to carry out an action unintentionally. For example, hacker change the email victim to their account, change password, or to make a funds transfer.

### How does CSRF Work
There is three condition to make the attack work:
1. **A relevant action** : the action in web application should be has reason to induce, such as privilage action that modifying role, or change email action.
2. **Cookie-based session handling** : To perform the attack, the victim user should have log in or has the session to target website. So when the performing csrf, the request that induced would be accepted
3. **No unpredictable request parameters** : the request that perform csrf didn't contain any parameter that has to be guess or manual input, such as old email or old password.

Example
```
POST /email/change HTTP/1.1 
Host: vulnerable-website.com 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 30 
Cookie: session=yvthwsztyeQkAPzeQ5gHgTvlyxHfsAfE 

email=wiener@normal-user.com
```

With the example request above, attacker can construct a web page that contain CSRF payload, like this

```
<html> 
	<body> 
		<form action="https://vulnerable-website.com/email/change" method="POST"> 
			<input type="hidden" name="email" value="pwned@evil-user.net" /> 
		</form> 
		<script>
			document.forms[0].submit();
		</script>
	</body>
</html>
```

The delivery mechanisms for cross-site request forgery attacks are essentially the same as for reflected XSS. Typically, the attacker will place the malicious HTML onto a website that they control, and then induce victims to visit that website. This might be done by feeding the user a link to the website, via an email or social media message. Or if the attack is placed into a popular website (for example, in a user comment), they might just wait for users to visit the website.

### Same Site
SameSite is a browser security mechanism that determines when a website's cookies are included in requests originating from other websites

There are three level on same site:
- Strict
  If a cookie is set with the `SameSite=Strict` attribute, browsers will not send it in any cross-site requests. In simple terms, this means that if the target site for the request does not match the site currently shown in the browser's address bar, it will not include the cookie.
  
- Lax
  `Lax` SameSite restrictions mean that browsers will send the cookie in cross-site requests, but only if both of the following conditions are met:
  - The request uses the `GET` method.
  - The request resulted from a top-level navigation by the user, such as clicking on a link.
  This means that the cookie is not included in cross-site `POST` requests, for example. As `POST` requests are generally used to perform actions that modify data or state (at least according to best practice), they are much more likely to be the target of CSRF attacks.
  
- None
  If a cookie is set with the `SameSite=None` attribute, this effectively disables SameSite restrictions altogether, regardless of the browser. As a result, browsers will send this cookie in all requests to the site that issued it, even those that were triggered by completely unrelated third-party sites.