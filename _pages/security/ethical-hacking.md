## TCP Flag Definitions

- SYN - The beginning of connection
- ACK - Acknowledgment of a previous packet
- FIN - CLose a TCP Connection
- RST - Abort a connection

## nmap

```shell
# Scanning a host
nmap -sT -p 21 192.168.1.1        # tcp ping
nmap -sP 192.168.1.1              # ping
nmap -sR 192.168.1.1              # RPC service
sudo nmap -sS 192.168.1.1         # SYN Scan
sudo nmap -sA 192.168.1.1         # ACK Scan
sudo nmap -sF 192.168.1.1         # FIN Scan
sudo nmap -sX 192.168.1.1         # Xmas Scan
sudo nmap -sN 192.168.1.1         # Null Scan to turn off all flags

sudo nmap -sU 192.168.1.1         # UDP Scan, will take time
sudo nmap -sO 192.168.1.1         # Protocol scanning, will take time
sudo nmap -r 192.168.1.1          # disable randomizing ports
sudo nmap -O vm1                  # OS Fingerprint (detect OS)
```
