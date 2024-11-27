
# Lab #2,22110081, Nguyen Hai Trieu, INSE330380E_03FIE

**Question 1**: 
Setup a set of vms/containers in a network configuration of 2 subnets (1,2) with a router forwarding traffic between them. Relevant services are also required:
- The router is initially can not route traffic between subnets
- PC0 on subnet 1 serves as a web server on subnet 1
- PC1,PC2 on subnet 2 acts as client workstations on subnet 2
  
**Answer 1**:

## 1. Create a network with router, 2 subnet using docker-compose.yml:
*docker-compose.yml
![image](https://github.com/user-attachments/assets/8b620ba4-4100-4f30-8ce4-b364057cc0c7)
*dockerfile
![image](https://github.com/user-attachments/assets/2624525a-3e7a-4ffb-be89-83ce30c2cbde)
*Then build using command : docker-compose up --build


**Question 2**:
- Enable packet forwarding on the router.
- Deface the webserver's home page with ssh connection on PC1
**Answer 2**:
##1. Enable packet forwarding :
*Using iptables to do this
![image](https://github.com/user-attachments/assets/c3968eff-9fae-4f7a-90e5-65703fecb726)
##2.Deface webserver's home page with ssh connection on pc1:
  Run ssh to connect to pc0 :
  ![image](https://github.com/user-attachments/assets/c722d0b1-bfa4-4c8c-99d8-35e41ec99c36)
 
**Question 3**:
  Config the router to block ssh to web server from PC1, leaving ssh/web access normally for all other hosts from subnet 1.   
  

**Answer 3**:
Using iptables to block ssh to web server from PC1 on router :
![image](https://github.com/user-attachments/assets/17435a55-01de-4831-98e7-10bcda340a91)

**Question 4**:
- PC1 now servers as a UDP server, make sure that it can reply UDP ping from other hosts on both subnets.
- Config personal firewall on PC1 to block UDP accesses from PC2 while leaving UDP access from the server intact.
**Answer 4**:
**Ping PC1 using PC2
![image](https://github.com/user-attachments/assets/f4b7799c-611d-4272-9122-bdc7ed029012)
**Config personal firewall on PC1 to block UDP accesses from PC2  while leaving UDP access from the server intact :
  We need to install iptables on PC1 first since we haven't installed on docker-compose.yml :
  ![image](https://github.com/user-attachments/assets/84d27734-8163-495c-825d-131a2d6e82e5)
  Then we block UDP accesses from PC2 and leave UDP access from the server intact :
  iptables -A INPUT -p udp -s 172.19.0.3 -j DROP ( Here we will DROP all access from source 172.19.0.3 using port udp )
  # Allow UDP access from webserver
  iptables -A INPUT -p udp -s 172.18.0.2 -j ACCEPT ( ACCEPT all access from source 172.18.0.2 using port UDP )
# Task 2: Encrypting large message 
Use PC0 and PC2 for this lab 
Create a text file at least 56 bytes on PC2 this file will be sent encrypted to PC0
**Question 1**:
Encrypt the file with aes-cipher in CTR and OFB modes. How do you evaluate both cipher in terms of error propagation and adjacent plaintext blocks are concerned. 
**Answer 1**:
- Demonstrate your ability to send file to PC0 to with message authentication measure.
- Verify the received file for each cipher modes
- Encrypt the file
  First we create a file text more than 56 bytes :
  ![image](https://github.com/user-attachments/assets/4fff7731-bb9c-40bc-89ad-88b074e5ac37)
  Then encrypt the file using CTR Mode and OFB Mode using openssl :
  ![image](https://github.com/user-attachments/assets/58cd732f-2734-4f02-bc4a-5b60f680ee81)
  Then install Open-ssh client to send file ( this will help us to authenticate our message ):
  apt-get update && apt-get install -y openssh-client
  Then send it to PC0 using :
  scp ctr_encrypted.txt root@172.18.0.2:/root/
  scp ofb_encrypted.txt root@172.18.0.2:/root/
  Here is our encrypted file on PC0
   ![image](https://github.com/user-attachments/assets/3150aee7-31a8-4436-aee6-91a7a746314e)
  Compared with the encrypted file on PC2 :
    ![image](https://github.com/user-attachments/assets/7eca08f6-bb19-4221-afcb-6b7f7c2072bc)
    They are the same


**Question 2**:
- Assume the 6th bit in the ciphered file is corrupted.
- Verify the received files for each cipher mode on PC0

**Answer 2**:
First we use xxd to edit the 6th bit in the ciphered file to make it corrupted:
xxd ctr_encrypted.txt | sed 's/00000050: ..../00000050: 00/' | xxd -r > ctr_encrypted_corrupted.txt
xxd ofb_encrypted.txt | sed 's/00000050: ..../00000050: 00/' | xxd -r > ofb_encrypted_corrupted.txt
Then we have to send this file again to PC0 using scp
Verify the received files:

**Question 3**:
- Decrypt corrupted files on PC0.
- Comment on both ciphers in terms of error propagation and adjacent plaintext blocks criteria. 





