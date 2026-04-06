# Bandit Level 11 → Level 12

## **Goal**
* Find the password for the next level. The password is stored in the file `data.txt`, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions (ROT13).

## **Explanation**
* At first, the file looked readable, but the text didn’t make sense.  
* I noticed that all letters seemed shifted, looked exactly like the recommanded wikipedia paged about **ROT13**, a simple letter substitution cipher.  
* Since ROT13 is just a letter rotation, Linux has tools that can apply it easily, instead of decoding manually.  
* I learned that the `tr` command can translate letters, which is perfect for ROT13.
* This challenge was really interesting and fun , having to implement a cipher from the **1st century BC** , a special case of **Caesar cipher**.

## **Solution**
* ROT13 being a simple letter substitution cipher that replaces a letter with the 13th letter after it in the Latin alphabet the new alphabet starts at N goes to Z and then goes again from A to M.
* Applying the ROT13 to decode the file we get: `cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'`
* Thus the key is : 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4

---

# Bandit Level 12 → Level 13

## **Goal**
* The password for the next level is stored in the file `data.txt`, which is a **hexdump of a file that has been repeatedly compressed**.

## **Explanation**
* At first, `data.txt` looked unreadable, so my instinct was to convert the hexdump back to binary using `xxd -r`.  
* I couldn’t write directly in `/home/bandit12`, so I copied the file to a temporary directory to work safely.  
* After converting to binary, the file was still compressed. I used the `file` command to identify its type.  
* Then I had to **decompress multiple times**, following the hints from `file` output, until I finally got a readable file containing the password.  

* This level teaches:
  * Converting hexdumps to binary  
  * Identifying file types (`file`)  
  * Handling multiple compression formats (`gzip`, `bzip2`, `tar`)  
  * Chaining decompression commands in the correct order  

## **Solution**
### 1.Hexdump to binary
xxd -r dataaa.txt > data.bin
### 2.Check type
file data.bin
### 3.Decompress first gzip
mv data.bin data.gz
gunzip data.gz
...
### 8.Extract tar
mv data data.tar
tar -xf data.tar
### 9.Decompress final gzip
mv data8.bin data8.gz
gunzip data8.gz
### 10. Show password
cat data8
* In the end after lots of decompressing we get to the ASCII Text file and we can finally see the key to the next level
* The key being: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

---

# Bandit Level 13 → Level 14

## **Goal**
* The password for the next level is stored in `/etc/bandit_pass/bandit14`  
* It can **only be read by user bandit14**  
* Instead of a password, we are given a **private SSH key** to log in as bandit14  

## **Explanation**
* This level introduces **SSH key-based authentication** instead of passwords  
* At first, I tried to use the key directly on the Bandit machine, but it didn’t work properly ( `ssh -i sshkey.private bandit14@localhost` ).
* Issues like **wrong permissions**, **wrong host**, or SSH refusing the key can easily happen  
* So I decided to try a different approach: use the key locally on my own machine  

* This actually reflects a real-world scenario:
  * You receive a private key  
  * You use it from your own system to authenticate to a remote server  

## **Solution (My Working Method)**
* First what we need to do is to display the private key: `cat sshkey.private`
* We then copy it into a file on our own system `bandit14.key` , and we give it permisions using `chmod 600 bandit14.key`.
* After that we can finally do: `ssh -i bandit14.key bandit14@bandit.labs.overthewire.org -p 2220`
* The `-i` parameter specifies the file that contains the private key.
* In the end after I got logged in into the **bandit14** user I did: `cat /etc/bandit_pass/bandit14` to get the real password.
* The final key being: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

---

# Bandit Level 14 → Level 15

## **Goal**
* Retrieve the password for the next level by submitting the current level’s password to **port 30000 on localhost**

## **Explanation**
* At first, this level might seem confusing because there is no file to read from.  
* Instead, the password must be **sent somewhere**, not just read.  
* This introduces the idea of interacting with a **network service**.

* The command used here is `nc` (netcat), which is basically a tool that lets you:
  * open a connection to a host and port  
  * send data manually  
  * receive responses  

* Since the target is `localhost`, it means:
  * the service is running on the same machine  
  * no external connection is needed  

## **Solution**
* I opened a connection to port 30000: `nc localhost 30000`
* Entered the password from the previous level: MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
* A message appeared **Correct!** followed by the key to the next level.
* The key is: 8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

---

# Bandit Level 15 → Level 16

## **Goal**
Retrieve the password for the next level by submitting the current level’s password to **port 30001 on localhost using SSL/TLS encryption**.

---

## **Explanation**

* In the previous level, we could use `nc` (netcat) to connect to a local port and send the password in plain text ( `nc localhost 30000` )
* For this level, port 30001 requires SSL/TLS encryption. A plain nc connection will not work here because the server expects an encrypted connection.
* But we can use a flag for `nc` to make it work if we check the manual we find that there is an `--ssl` option for it.
* SSL/TLS is the protocol used for secure communication (like HTTPS). So we need a tool that can speak SSL/TLS to the server.
* The Linux command used for this is `nc --ssl`

## **Solution**
* We can use `nc -ssl localhost 30001` , after that we just enter the password **8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo**
* We received the new key: kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

---

# Bandit Level 16 → Level 17

## Goal
- The credentials for the next level can be retrieved by submitting the password of the current level to a port on **localhost** in the range **31000-32000**.  
- First, find out **which ports have a server listening** on them.  
- Then figure out **which of those use SSL/TLS** and which don’t.  
- Only **one server** will give the next credentials; the others just echo back what you send.  

## Explanation
This level introduces **network reconnaissance** combined with **SSL/TLS connections**:
* We scan the port range to see which ports are open: `nmap -p 31000-32000 localhost`
The output received:
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
* To determine which open port requires SSL/TSL we do : `ncat --ssl localhost port` , where port is one of the open ports.
* We test on them and in the end we find out that the one that requires is : **port 31790**
* After we enter the correct password , we receive an RSA private key that we must use to login into the next level
* We save it into our machine in a file as **key** , we copy the RSA contents into it and after we save.
* To connect to the next level we use : `ssh -i key bandit17@bandit.labs.overthewire.org -p 22200`

