# Ubuntu-SSH-Server-Monitoring-Lab
Hands-on Ubuntu Server lab covering openSSH in Ubuntu Server set-up, remote excess from Window Power Shell, SSH key pair prepsration and linux connection/ session monitoring
Project Overview
A beginner-level defensive Linux administration lab using an Ubuntu Server virtual machine in VirtualBox and Windows PowerShell as the SSH client.
The objective was to configure remote SSH access, prepare SSH public-key authentication, and practise basic session and connection monitoring commands commonly useful in Linux administration and SOC operations.
Scope: All activities were performed in a self-owned local VirtualBox lab. This is a learning exercise, not a production-server assessment or a confirmed security incident investigation.
Lab Environment
Component
Configuration
Server
Ubuntu Server VM
Client
Windows PowerShell
Virtualization
Oracle VirtualBox
Network mode
NAT with port forwarding
SSH service
OpenSSH Server
SSH host-side port
2222
SSH guest-side port
22
Ubuntu account
vikrant
Practical Work Completed
1. OpenSSH Server setup (Ubuntu)
Installed and started the SSH service:
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
systemctl status ssh
Verification: Confirmed that the SSH service was enabled and running.
2. VirtualBox NAT port forwarding
Configured a TCP port-forwarding rule:
Host IP: 127.0.0.1
Host port: 2222
Guest IP: 10.0.2.15
Guest port: 22
This allowed the Windows host to connect to the Ubuntu VM through the forwarded port.
3. Remote SSH connection (Windows PowerShell)
Connected from Windows PowerShell:
ssh -p 2222 vikrant@127.0.0.1
Result: Successfully established an SSH session to the Ubuntu VM.
4. SSH key-pair preparation (Windows PowerShell)
Generated an Ed25519 key pair:
ssh-keygen -t ed25519 -C "ubuntu-server"
The public key was then appended to the Ubuntu account's ~/.ssh/authorized_keys file using an SSH command.
Security note: The private key remains on the Windows client. Only the public key is placed on the Ubuntu server. Never publish or share the private key.
5. SSH session and connection monitoring (Ubuntu)
Practised the following commands:
who
w
sudo ss -tnp state established
sudo ss -tnp | grep ':22'
last
last -n 10
sudo journalctl -u ssh --since "1 hour ago"
Command
Purpose
who
Lists currently logged-in sessions
w
Shows logged-in users and session activity
ss -tnp state established
Displays established TCP connections and process details where available
ss -tnp | grep ':22'
Helps inspect TCP socket entries involving port 22
last
Displays recorded login history
last -n 10
Displays the latest 10 login records
journalctl -u ssh --since "1 hour ago"
Reviews recent SSH service logs
Security Concepts Practised
SSH provides an encrypted channel for remote administration.
TCP port 22 is the default SSH service port.
Ed25519 is a modern SSH key type.
The client keeps the private key; the server stores the trusted public key.
~/.ssh should normally have restrictive permissions (700), and authorized_keys should normally be restricted (600).
ESTAB means a TCP connection is established; it does not by itself prove that a session is malicious or that authentication succeeded.
Session listings, socket information, login history, and SSH service logs provide different views and should be correlated rather than interpreted in isolation.
Verification Status / Limitations
OpenSSH service status: verified running.
Windows-to-Ubuntu SSH connection: verified.
Public key deployment command: executed.
Public-key-only login: still needs a separate test using the private key from Windows PowerShell. Do not claim key-based authentication is fully verified until that test succeeds.
No suspicious activity or security incident was identified or claimed in this lab.
Next Steps
Test key-based SSH login from Windows PowerShell.
Confirm the expected SSH authentication method in the client output and server logs.
Review SSH configuration safely while keeping a working access method available.
Continue with basic server hardening and, later, Nginx and HTTPS fundamentals.
Skills Demonstrated
Ubuntu Linux · Windows PowerShell · SSH · OpenSSH · Ed25519 · VirtualBox NAT · Linux Service Management · TCP Socket Monitoring · Linux Login Auditing · SSH Log Review
Disclaimer: This repository documents a personal learning lab on systems owned and controlled by the author. It is intended for educational and defensive administration practice.
