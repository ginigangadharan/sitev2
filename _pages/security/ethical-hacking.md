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
sudo nmap -sS 192.168.1.1         # SYN Scan, need root privilege
sudo nmap -sA 192.168.1.1         # ACK Scan, need root privilege
sudo nmap -sF 192.168.1.1         # FIN Scan, need root privilege
```
