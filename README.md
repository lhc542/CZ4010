# Implementation of a Certificate Authority(CA) in Python
This project aims to demonstrate the implementation of a certificate authority in Python. This README contains a brief introduction on the context, motivation for the project, design and implementation of CA and a discussion.
## 1. Introduction to Certificate Authority 
### Digital Certificate
A Digital Certificate is an electronic "password" that allows a person, organizaion to exchange data securely over the Internet using the public key infrastructure (PKI). Digital Certificate is also known as a public key certificate or identity certificate.
### Certificate Authority
Certificate authority is a TRUSTED entity that issue digital certificates.  

### Steps in generating a digital certificate:
1. The User randomly generate a public and private key pair using safe algorithms(RSA with 2048 bit key length and 65537 as encrypting exponent in this project)
2. The User sends a certificate signing request(CSR) which includes public key + user identity to CA
3. CA verify user identity and build a certificate for User
4. certificate contains user public key, user identity, and CA's signature using its private key.
5. Others can verify the certificate by CA's signature and its public key.

### Hierarchical Certificate Authority
A root CA certify other Intermediate CAs and Intermediate CAs certify end-users. This project implements a 2-level CA as seen below.  
![image](https://user-images.githubusercontent.com/58136973/143768032-0d929a8c-ed69-4f86-87be-d619e5bed700.png)  
Advantages:
1. Set appropriate level of security to each layer
2. Division of administrative tasks for better load balancing and security
3. Enable different validity period for different level!

## 2. Project Motivation
### Be your own CA
Usually certificates are signed by trust-worthy authorities(for example, IdenTrust and DigiCert) to ensure that others also trust this certificate. However, sometimes it is useful to create self-signed certificate as it might be costly to get a trusted certificate. This is especially useful for demonstration and testing purposes.
### Implementation in Python
There are existing software available(For example, OpenSSL) for easy implementation of Certificate Authority on desktop. However, being one of the most commonly-used programming languages worldwide, Implementing CA in Python will surely benifit more people as it allows easy implementation of CA without the need of downloading additional softwares, and also allows easy integration with other existing python projects.  
  
This project is implemented using Python Cryptography library, which is based on OpenSSL functions.

## 3. Design and implementation
### Cryptography library:  
cryptography includes both high level recipes and low level interfaces to common cryptographic algorithms such as symmetric ciphers, message digests, and key derivation functions. https://cryptography.io/en/latest
  
### X.509:  
X.509 is an ITU-T standard for a public key infrastructure. https://cryptography.io/en/latest/x509/
  
### generate_root_ca(name,password):  
Generate self-signed root certificate using "name" as the name indicated in the certificate as well as file names, "password" as the password for symmetric encryption of the private key of root CA.  
At the same time, create an empty Certificate Revocation List(CRL) for future revocation.  
  
### generate_intermediate_ca(name,password,ca_name,ca_password):  
Generate intermediate certificate signed by root certificate.  using "ca_name" as the name indicated in the certificate as well as file names, "ca_password" as the password for symmetric encryption of the private key of root CA. "name" and "password" refer to that of root CA to get its private key for signature.  
  
### issue_cert(name,ica,ica_password)  
Generate client certificate signed by intermediate CA.  using "name" as the name indicated in the certificate as well as file names, without encryption to the private key. "ica" and "ica_password" refer to that of intermediate CA to get its private key for signature.   
  
### revoke_cert(name,password)  
Revoke issued certificate. "name" indicates the certificate to be revoked and "password" is that of the root CA to generate another CRL object.  
  
### verify_cert(issuer,certname)  
Verify a certificate with its issuer. "certname" indicates the certificate to be verified and "issuer" indicates the CA that issue this certificate. This function will be called twice tro verify both client certificate with ica and ica with root ca  
  
### Main driver:  
1.Generate root CA  
2.generate intermediate CA  
3.Issue a certificate  
4.Verify a certificate  
5.Revoke a certificate  

## 4. Discussion
This project is a simple implementation of CA for demonstrating purposes only. The codes are not suitable for industrial and commercial usage due to the following aspects:  
### Trust-worthy
This implementation generated a root CA that is self-signed. This means that there is no other trusted 3rd party who sign this certificate and other entities will not trust this certificate. For commercial purposes, one need to generate key pairs and send to trusted CAs to get a certificate.
### Security
Furthermore, the verification of certificate in this project is not secured. This is due to the fact that verification of certificate is a much more complex problem than simple matching between the signature and CA's public key. It needs to consider many other information such as User's identity, validation period, dates of creation...etc  
For better security, implement with external libraries such as https://github.com/wbond/certvalidator
