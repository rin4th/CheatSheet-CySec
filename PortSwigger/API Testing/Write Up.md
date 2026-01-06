### Exploiting an API endpoint using documentation
**Payload** :
set the endpoint to /api, it will reveal all the api. try GET /api/user/carlos to get carlos's information. then DELETE /api/user/carlos

### Finding and exploiting an unused API endpoint
**Flow** :
on `/price` endpoint trying to use other method, such as `POST`, then on header response will reveal allowed method, which allow PATCH. what we think when we can `PATCH` on `/price` endpoint, yes. we can change the price to $0. by add `Content-Type: applicaiton/json` and body request `price` to 0, we can buy the item

### Exploiting a mass assignment vulnerability
**Payload** :
```json
{ "chosen_discount":{"description":null,"discount_id":null,"percentage":100},"chosen_products":[{"product_id":"1","quantity":1}]}
```
First check the `/api` endpoint, found out that on `POST /checkout` there's another param `chosen_discount` and tell us what values ​​can be filled in, so intercept while checkout add param `chosen_discount` fill `percentage` param to 100 so we can buy it

### Exploiting server-side parameter pollution in a query string
**Payload** :
```html
csrf=Z9Ht8Aq3UKApgcTbJwRGEdkrp4lt2cRK&username=administrator%26field=reset_token%23
```
while `&username=administrator%23` server return error `Field not specified`, which mean it's like there's parameter with name `field`, so we can use that parameter to retrieve needed value. which value `reset_token` what we need
