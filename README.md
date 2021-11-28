# Implementation of a Certificate Authority(CA) in Python
This project aims to demonstrate the implementation of a certificate authority in Python. This README contains a brief introduction on the context, motivation for the project, design and implementation of CA and a discussion.
# 1. Introduction to Certificate Authority 
# Digital Certificate
A Digital Certificate is an electronic "password" that allows a person, organizaion to exchange data securely over the Internet using the public key infrastructure (PKI). Digital Certificate is also known as a public key certificate or identity certificate.
# Certificate Authority
Certificate authority is a TRUSTED entity that issue digital certificates.  

# Steps in generating a digital certificate:
1. The User randomly generate a public and private key pair using safe algorithms(RSA with 2048 bit key length and 65537 as encrypting exponent in this project)
2. The User sends a certificate signing request(CSR) which includes public key + user identity to CA
3. CA verify user identity and build a certificate for User
4. certificate contains user public key, user identity, and CA's signature using its private key.
5. Others can verify the certificate by CA's signature and its public key.

# Hierarchical Certificate Authority
A root CA certify other Intermediate CAs and Intermediate CAs certify end-users.  
![image](https://user-images.githubusercontent.com/58136973/143768032-0d929a8c-ed69-4f86-87be-d619e5bed700.png)  
Advantages:
1. Set appropriate level of security to each layer
2. Division of administrative tasks for better load balancing and security
3. Enable different validity period for different level!

# 2. Project Motivation
# Be your own CA
Usually certificates are signed by trust-worthy authorities(for example, IdenTrust and DigiCert) to ensure that others also trust this certificate. However, sometimes it is useful to create self-signed certificate as it might be costly to get a trusted certificate. This is especially useful for demonstration and testing purposes.
# Implementation in Python
There are existing software available(For example, openSSL) for easy implementation of Certificate Authority on desktop. However, being one of the most commonly-used programming languages worldwide, Implementing CA in Python will surely benifit more people as it allows easy implementation of CA without the need of downloading additional softwares, and also allows easy integration with other existing python projects.  
  
This project is implemented using Python Cryptography library, which is based on OpenSSL functions.
