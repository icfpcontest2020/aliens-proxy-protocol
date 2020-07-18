# Alien Proxy Protocol

Alien Proxy allows you to send requests to Aliens using our antenna.
It's a simple HTTP server.

## Base Url

https://icfpc2020-api.testkontur.ru/
 
## Authorization

Pass your team API Key in query string.

Example:
```
POST /some_request?apiKey=<your_team_api_key>
```

## Send request to Aliens

Use this to send new request to Aliens.
Pass modulated string in the request body with `text/plain` Content-Type.

Relative Url: `/aliens/send`

Request example:
```
POST /aliens/send?apiKey=<your_team_api_key> HTTP/1.1

1101000
```

### Possible responses

#### 200 OK

You will get this response if the Aliens answer fast enough.
 
Response body will contain modulated Aliens' response with `text/plain` Content-Type.

Response example:    
```
HTTP/1.1 200 OK

1101100001110111110111101010101011100
```
    
#### 302 Found

You will get this response if the Aliens didn't answer fast enough.
     
If the Aliens didn't respond fast enough we respond with `302 Found`.
The `Location` response header will contain url where you can ask aliens response later.
In fact, this location will always contain `/aliens/{responseId}?apiKey=<your_team_api_key>`.
It's a long-polling protocol, so you can make a new request to this location immediately after you got it.
Many HTTP client implementations, e.g. C# HttpClient, can follow redirects automatically, so you don't deal with this.

Response example:
```
HTTP/1.1 302 Found
Location: /aliens/{75960227-653C-47E3-A47A-118A46AFFD4C}?apiKey=<your_team_api_key>
```

## Get response from Aliens

Use this to get a response to the request you have sent earlier,
in a case when Aliens didn't respond fast enough.

Relative Url: `/aliens/{responseId}`

Possible responses are the same as in `/aliens/send`.
