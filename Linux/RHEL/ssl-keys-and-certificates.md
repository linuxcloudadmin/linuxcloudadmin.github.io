[Back to Homepage](https://linuxcloudadmin.github.io)

## SSL Keys and Certificate

- A browser requests a secure page (usually https://) from a web server.  
- The web server sends its public key with its certificate (contains certificate holder, issuer, expiry details, public key, digital signature). 
- The browser checks that the certificate was issued by a trusted party (usually a trusted root CA), that the certificate is still valid and that the certificate  
is related to the site contacted. 
- The browser then uses the public key, to encrypt a random symmetric encryption key and sends it to the server with the encrypted URL required as well as other 
encrypted http data. 
- The web server decrypts the symmetric encryption key using its private key and uses the symmetric key to decrypt the URL and http data.  
- The web server sends back the requested html document and http data encrypted with the symmetric key.  
- The browser decrypts the http data and html document using the symmetric key and displays the information. 

### Private Key/Public Key

- The encryption using a private key/public key pair ensures that the data can be encrypted by one key but can only be decrypted by the other key pair.  
- The keys are similar in nature and can be used alternatively: what one key encrypts, the other key pair can decrypt. 
- The key pair is based on prime numbers and their length in terms of bits ensures the difficulty of being able to decrypt the message without the key pairs. 
- The trick in a key pair is to keep one key secret (the private key) and to distribute the other key (the public key) to everybody.  
- Anybody can send you an encrypted message, that only you will be able to decrypt. You are the only one to have the other key pair, right? 
- In the opposite,  you can certify that a message is only coming from you, because you have encrypted it with you private key, and only the associated public key will decrypt it  
correctly. Beware, in this case the message is not secured you have only signed it. Everybody has the public key, remember! 

## Certificates

- How do you know that you are dealing with the right person or rather the right web site ? Well, someone has taken great length (if they are serious) to ensure that the web site owners are who they claim to be. 
- This someone, you have to implicitly trust: you have his/her certificate loaded in your browser (a root Certificate). 
- A certificate, contains information about the owner of the certificate, like e-mail address, owner's name, certificate usage, duration of validity, resource location or Distinguished Name (DN) which includes the Common Name (CN) (web site address or e-mail address depending of the usage) and the certificate ID of the person who certifies (signs) this information. 
- It contains also the public key and finally a hash to ensure that the certificate has not been tampered with. As you made the choice to trust the person who signs this certificate, therefore you also trust this certificate. This is a certificate trust tree or certificate path. Usually your browser or application has already loaded the root certificate of well known Certification Authorities (CA) or root CA Certificates. 
- The CA maintains a list of all signed certificates as well as a list of revoked certificates. A certificate is insecure until it is signed, as only a signed certificate cannot be modified. You can sign a certificate using itself, it is called a self signed certificate. All root CA certificates are self signed.


[Back to Homepage](https://linuxcloudadmin.github.io)
