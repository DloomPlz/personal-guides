####Julia Evans Curl Exercices

#####Request https://httpbin.org

```curl https://httpbin.org```

#####Request https://httpbin.org/anything. httpbin.org/anything will look at the request you made, parse it, and echo back to you what you requested. curl’s default is to make a GET request.

```curl https://httpbin.org/anything```

#####Make a POST request to https://httpbin.org/anything

```curl https://httpbin.org/anything -X POST```

#####Make a GET request to https://httpbin.org/anything, but this time add some query parameters (set value=panda).

```curl https://httpbin.org/anything --data '{"value": "panda"}```

#####Request google’s robots.txt file (www.google.com/robots.txt)

```curl www.google.com/robots.txt```

#####Make a GET request to https://httpbin.org/anything and set the header User-Agent: elephant.

```curl https://httpbin.org/anything -H "User-Agent: elephant"```

#####Make a DELETE request to https://httpbin.org/anything

```curl https://httpbin.org/anything -X DELETE```

#####Request https://httpbin.org/anything and also get the response headers

```curl https://httpbin.org/anything -i```

#####Make a POST request to https://httpbin.com/anything with the JSON body {"value": "panda"}

```curl https://httpbin.org/anything -X POST --data '{"value": "panda"}'```

#####Make the same POST request as the previous exercise, but set the Content-Type header to application/json (because POST requests need to have a content type that matches their body). Look at the json field in the response to see the difference from the previous one.

```curl https://httpbin.org/anything -X POST -H "Content-Type: application/json" --data '{"value": "panda"}'```

Renvoie une vraie valeur lorsque le header est spécifié

Pas specifié : 
```
"json": null,
```

Spécifié :
```
"json": {
    "value": "panda"
  },
```

#####Make a GET request to https://httpbin.org/anything and set the header Accept-Encoding: gzip (what happens? why?)

```curl https://httpbin.org/anything -H "Accept-Encoding: gzip"```

Réponse zippé?

#####Put a bunch of a JSON in a file and then make a POST request to https://httpbin.org/anything with the JSON in that file as the body

```curl https://httpbin.org/anything -X POST --data @example.json```

#####Make a request to https://httpbin.org/image and set the header ‘Accept: image/png’. Save the output to a PNG file and open the file in an image viewer. Try the same thing with with different Accept: headers.

```curl https://httpbin.org/image -H "Accept: image/png" > image.png```

#####Make a PUT request to https://httpbin.org/anything

``` curl https://httpbin.org/anything -X PUT```

#####Request https://httpbin.org/image/jpeg, save it to a file, and open that file in your image editor.

```curl https://httpbin.org/image/jpeg -H "Accept: image/jpg" > image.jpg```

#####Request https://www.twitter.com. You’ll get an empty response. Get curl to show you the response headers too, and try to figure out why the response was empty.

``` curl https://www.twitter.com -i```

Réponse 301 : réponse vide : Moves Permanently
Il faut etre redirigé via le header location

#####Make any request to https://httpbin.org/anything and just set some nonsense headers (like panda: elephant)

```curl https://httpbin.org/anything -X POST -H "panda: elephant"```

#####Request https://httpbin.org/status/404 and https://httpbin.org/status/200. Request them again and get curl to show the response headers.

```curl https://httpbin.org/status/404 -i
HTTP/1.1 404 NOT FOUND
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: text/html; charset=utf-8
Date: Wed, 28 Aug 2019 08:02:48 GMT
Referrer-Policy: no-referrer-when-downgrade
Server: nginx
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Content-Length: 0
Connection: keep-alive
```

```curl https://httpbin.org/status/200 -i
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Content-Type: text/html; charset=utf-8
Date: Wed, 28 Aug 2019 08:03:05 GMT
Referrer-Policy: no-referrer-when-downgrade
Server: nginx
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Content-Length: 0
Connection: keep-alive
```

#####Request https://httpbin.org/anything and set a username and password (with -u username:password)

```curl https://httpbin.org/anything -u "username:password"```

#####Download the Twitter homepage (https://twitter.com) in Spanish by setting the Accept-Language: es-ES header.

```curl https://twitter.com -H "Accept-Language: es-ES" > twitterES.html```

#####Make a request to the Stripe API with curl. (see https://stripe.com/docs/development for how, they give you a test API key). Try making exactly the same request to https://httpbin.org/anything.

```
curl https://api.stripe.com/v1/charges \
-u sk_test_4eC39HqLyjWDarjtT1zdp7dc: \
-d amount=2000 \
-d currency=usd \
-d customer=cus_FhlnSJamFioqyT
```

