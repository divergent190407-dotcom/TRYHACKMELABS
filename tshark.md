- https://tryhackme.com/room/tshark?utm_campaign=social_share&utm_medium=social&utm_content=share-completed-room&utm_source=copy&sharerId=6940f9fd986dfc95caf4ed83
- download with sudo apt install tshark
# to read a file with tshark

## List Network Interfaces

Command:
tshark -D

What this command means
tshark → starts the tshark program
-D → Display available interfaces
Your computer has multiple network interfaces.

Examples:

WiFi card
Ethernet adapter
Loopback interface

Tshark asks Linux:

“What network interfaces are available for packet capture?”

## Start DNS Capture

Command:

sudo tshark -i wlan0 -Y dns

-Breaking the command apart
- sudo : run as administrator or root
- tshark : launches tshark
- -iwlan0 : linsten to all the packages passing through wifi
-  -Y dns : display filter, shows only dns packets otherwise rthee are many tcp, https, arp, icmp,background system trafic

## to save then into file n then read

- command: 
 open second terminal as it will take time to capture the psckets as it is filtering them down.
  open another terminal n start ping google,com
  then do do ctr c to stop then ,,  sudo tshark -r /tmp/dns_capture.pcap -Y dns
  -r to read the files

## error
- wlan problem:
- i did not have wlan, instead it was eth0 to get all of these i need to do ifconfig. 
because i was working inside a virtualised environment that means ethrnet: eth0, wlan represents wifi interface.
- -y /-Y:
-  -y sets data link type, while -y applies a display filter

#  to read a file in kali

- open it inside the kali box, it will aotomatically open through wire shark.
## apply filters
- dns.qry.type==1 / dns.a for A type files
- dns to read all the dns
# through terminal
- firct find the file:
- └─$ find ~ -name "dns_exfil_1617459299197.pcap"
- go into that directory:
- cd ~/downloads
- then:
- └─$ tshark -r ~/Downloads/dns_exfil_1617459299197.pcap -Y "dns"
-└─$ tshark -r dns_exfil_1617459299197.pcap -Y "dns.flags.response==0" -T fields -e dns.qry.name
- $ tshark -r dns_exfil_1617459299197.pcap \                                 
- -Y "dns.flags.response==0" \
- -T fields -e dns.qry.name | cut -d'.' -f1

