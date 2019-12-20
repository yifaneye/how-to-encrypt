# encrypt
This is a repository for encrytion using OpenSSL

## Usage
1. Generate the private key using RSA
```
openssl genrsa -aes256 -out private.pem 2048
```
2. Generate the public key using RSA
```
openssl rsa -in private.pem -pubout > public.pem
```
3. Generate the session key for 256-bit AES in CBC mode
```
openssl enc -salt -aes-256-cbc -k keyrect -P > session.txt
cat session.txt
```
4. Hash the message using SHA256 and sign the digest using the private key
```
openssl dgst -sha256 -sign private.pem -out signature.txt message.txt
```
5. Encrypt the session key using the public key of the receiver
```
openssl rsautl -encrypt -pubin -inkey public2.pem -in session.txt -out session.enc
```
6. Encrypt the message using 256-bit AES in CBC mode with the session key
```
openssl enc -aes-256-cbc -salt -in message.txt -out message.enc
```
7. Encrypt the digest using 256-bit AES in CBC mode with the session key
```
openssl enc -aes-256-cbc -salt -in signature.txt -out signature.enc
```