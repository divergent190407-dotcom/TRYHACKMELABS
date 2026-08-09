### 1. First check whether the web server responds

Run:

```bash
curl -i http://10.49.165.130/
```

### 2. Then run Gobuster with more useful output

```bash
gobuster dir -u http://10.49.165.130/ \
-w /usr/share/wordlists/dirb/common.txt \
-x php,html,txt \
-t 30
```

If that still immediately says `Finished` with nothing found, try:

```bash
gobuster dir -u http://10.49.165.130/ \
-w /usr/share/wordlists/dirb/common.txt \
-v
```

### 3. Also check whether the service is actually on port 80

```bash
nmap -Pn -sV 10.49.165.130
```

If Nmap shows something like:

```text
8000/tcp open  http
```

then your Gobuster target needs to be:

```bash
gobuster dir -u http://10.49.165.130:8000/ \
-w /usr/share/wordlists/dirb/common.txt
```
