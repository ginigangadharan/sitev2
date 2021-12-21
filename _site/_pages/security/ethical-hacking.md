
- [Tools](#tools)
  - [Nessus](#nessus)

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

nmap -sP 192.168.1.0/24           # Network scanning
nmap -sP 192.168.1.1-10.          # Network scanning
```
## Packet Sniffers

Applications that monitor, filter and capture data packets transferred over network. (libpcap/winpcap is library commonly used by sniffers)

- Network Monitor
- tcpdump

**Tools**

- Wireshark
- tcpdump/windump
- snort
- suricata
- Zeek
- Kismet

## Blackbox Testing 

- Pentest, EH

## Whitebox Testing

- System Analysis

# Tools

## Nessus

**Nessus** is a proprietary vulnerability scanner developed by Tenable, Inc. ([Download](https://www.tenable.com/products/nessus))

```shell
$ sudo service nessusd start

# Reset Nessus Password
/opt/nessus/sbin/nessuscli chpasswd
/opt/nessus/sbin/nessuscli lsuser

```

Nessus only supports RSA/DSA key types for the authentication.

```shell
ssh-keygen -m PEM -t rsa
# or Convert the OPENSSH key to .PEM:
ssh-keygen -p -m PEM -f /path/to/private_key
```

