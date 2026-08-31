## 1. Nmap — "What is running?"

### 🔍 When to use it

You have a target IP and don't know what services are exposed.

### Command

```bash
nmap -sC -sV <TARGET_IP>
```

Example:

```bash
nmap -sC -sV 10.10.10.10
```

### If you want all ports

```bash
nmap -p- --min-rate 1000 <TARGET_IP>
```

Then investigate discovered ports:

```bash
nmap -sC -sV -p <PORTS> <TARGET_IP>
```

**Trigger in your brain:**

> "I have an IP. What's running on it?" → **Nmap**

---

# 2. Browser / Manual Recon — "What does the application reveal?"

### 🔍 When to use it

You find HTTP/HTTPS running.

Check:

```text
/
/robots.txt
/sitemap.xml
```

Example:

```bash
curl -i http://<TARGET_IP>/
```

Check headers:

```bash
curl -I http://<TARGET_IP>/
```

**Trigger:**

> "I found a website. Let me understand it before fuzzing." → **Browser + curl**

---

# 3. Gobuster — "Are there hidden directories?"

### 🔍 When to use it

The website works, but you've run out of visible links and want to discover additional content.

```bash
gobuster dir \
-u http://<TARGET_IP>/ \
-w /usr/share/seclists/Discovery/Web-Content/common.txt
```

If you want specific extensions:

```bash
gobuster dir \
-u http://<TARGET_IP>/ \
-w /usr/share/seclists/Discovery/Web-Content/common.txt \
-x php,html,txt
```

**Trigger:**

> "There must be pages/directories that aren't linked." → **Gobuster**

---

# 4. ffuf — "I need flexible fuzzing"

### 🔍 When to use it

You want to fuzz:

* Directories
* Files
* Parameters
* Virtual hosts

Directory discovery:

```bash
ffuf -u http://<TARGET_IP>/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/common.txt
```

File extensions:

```bash
ffuf -u http://<TARGET_IP>/FUZZ.php \
-w /usr/share/seclists/Discovery/Web-Content/common.txt
```

**Trigger:**

> "I need to systematically test many possible values." → **ffuf**

---

# 5. Burp Repeater — "What happens if I change this?"

### 🔍 When to use it

You found:

* Forms
* POST requests
* Cookies
* Parameters
* API requests

Workflow:

```text
Intercept request
      ↓
Send to Repeater
      ↓
Change ONE thing
      ↓
Send
      ↓
Compare responses
```

Example:

```http
GET /profile?id=5 HTTP/1.1
Host: <TARGET>
```

You might change the ID **within an authorized lab**:

```http
GET /profile?id=6 HTTP/1.1
Host: <TARGET>
```

**Trigger:**

> "What happens if I modify this request?" → **Repeater**

---

# 6. Login Form — "How does authentication work?"

### 🔍 First use

Capture the login request using Burp.

Look for:

```text
POST /login
username=
password=
```

You can also inspect with:

```bash
curl -i -X POST \
-d "username=test&password=test" \
http://<TARGET_IP>/login
```

**Trigger:**

> "I found authentication. I need to understand the request first." → **Burp**

---

# 7. Hydra — "An authorized lab explicitly requires credential testing"

### 🔍 When to use it

Only after you know:

* The username (or allowed username list)
* Login endpoint
* POST parameter names
* What indicates failure

Example pattern:

```bash
hydra -l <USERNAME> \
-P <WORDLIST> \
<TARGET_IP> \
http-post-form \
"<PATH>:username=^USER^&password=^PASS^:F=Invalid credentials"
```

Example:

```bash
hydra -l admin \
-P /usr/share/wordlists/rockyou.txt \
10.10.10.10 \
http-post-form \
"/login:username=^USER^&password=^PASS^:F=Invalid credentials"
```

**Trigger:**

> "The lab explicitly authorizes credential brute-force testing, and I know the request format." → **Hydra**

---

# 8. Search Box / URL Parameter — "What type of input is this?"

Example:

```text
/search?q=test
/product?id=5
/profile?user=admin
```

Use Burp Repeater first.

Test harmlessly:

```text
Normal input → Does it work?

Different input → What changes?

Special characters → Is input handled safely?
```

Then classify the functionality:

| Parameter       | Think about           |
| --------------- | --------------------- |
| `id=5`          | Access control / IDOR |
| `search=test`   | Input handling        |
| URL parameter   | SSRF possibilities    |
| Filename/path   | Path traversal/LFI    |
| HTML reflection | XSS                   |

**Trigger:**

> "User input controls something." → **Understand the input before choosing a tool**

---

# 9. Nikto — "Quick web-server misconfiguration checks"

### 🔍 When to use it

After confirming a web server exists and you want an automated overview of common issues.

```bash
nikto -h http://<TARGET_IP>
```

For HTTPS:

```bash
nikto -h https://<TARGET_IP>
```

**Trigger:**

> "I've enumerated manually; now I want an additional automated sanity check." → **Nikto**

---

# 🔥 The complete order I recommend

```text
1. Target IP
      ↓
2. Nmap
      ↓
3. Find HTTP/HTTPS
      ↓
4. Open website manually
      ↓
5. robots.txt / source code / JavaScript
      ↓
6. Gobuster or ffuf
      ↓
7. Find functionality
      ↓
8. Burp Suite
      ↓
9. Ask:
      "What does this parameter/function control?"
      ↓
10. Choose the appropriate test
```
