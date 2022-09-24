# What is SSL?

SSL is the original name of the cryptographic protocol for authenticating and
encrypting communications over a network. Officially, SSL was replaced by an
updated protocol called TLS some time ago. 

TLS is simply a newer version of SSL. However, most people still say SSL
instead of TLS. SSL and TLS serve the same purpose, protecting sensitive
information during transmission, but under the hood, the cryptography has
changed a lot from the original SSL to the latest TLS v1.3.  

Digital certificates are the core of the SSL protocol; they initiate the secure
connections between servers (e.g., websites, intranets, or VPN) and
clients(e.g., web browsers, applications, or email clients).

SSL certificates offer adequate protection against phishing and eavesdropping
of transmissions and automatic authentication of a server, such as a website
domain. If a website asks for users’ sensitive information, it needs to have an
SSL certificate to encrypt it during transmission. If there is no SSL
certificate, then that connection should not be trusted with any private
information.

# How does it Work?

The primary purpose of SSL is to provide a secure transport-layer connection
between two endpoints, the server and the client. This connection is typically
between a website server and the client’s browser, or a mail server and the
client’s email application, such as Outlook. 

SSL comprises two separate protocols: 

1. The Handshake protocol authenticates the server(and optionally the client),
   negotiates crypto suites, and generates the shared key. 
1. The Record protocol isolates each connection and uses the shared key to
   secure communications for the remainder of the session. 

## The Handshake Protocol

The SSL handshake is an asymmetric cryptography process for establishing a
secure channel for server and client to communicate — HTTPS connections always
begins with the SSL handshake.

A successful handshake takes place behind the client’s browser or application,
instantly and automatically — without disturbing the client user experience.
However, A failed handshake triggers the termination of the connection, usually
preceded by an alert message in the client’s browser. 

Provided the SSL is valid and correct, the handshake offers the following
security benefits: 


* **Authentication:** The server is always authenticated for as long as the
  connection is valid. 
* **Confidentiality:** Data sent via SSL is encrypted and only visible to the
  server and client. 
* **Integrity:** Digital Certificate Signatures ensure the data has not been
  modified during the transfer.

In summary, SSL certificates fundamentally work using a blend of asymmetric
cryptography and symmetric cryptography for communications over the internet.
There are also other infrastructures involved in achieving SSL communication in
enterprises, known as Public Key Infrastructures.

# How do SSL Certificates Work?

When you receive the SSL certificate, you install it on your server. You can
install an Intermediate certificate that establishes your SSL certificate’s
credibility by chaining it to your CA’s root certificate.

Root certificates are self-signed and form the basis of an X.509-based
Public-Key Infrastructure (PKI). The PKI supporting HTTPS for secure web
browsing and electronic signature schemes depends on root certificates. In
other applications of X.509 certificates, a hierarchy of certificates certifies
a certificate’s issuance validity. This hierarchy is called a certificate
“Chain of Trust.”

## Chain of Trust

The Chain of Trust refers to your SSL certificate and its link to a trusted
certificate authority. For an SSL certificate to be trusted, it must trace back
to a trusted root CA. A Chain of Trust ensures privacy, trust, and security for
all parties involved.

At the core of every PKI is the root CA; it serves as the trusted source of
integrity for the entire system. The root certificate authority signs an SSL
certificate, thus starting the Chain of Trust. If the root CA is publicly
trusted, then any valid CA certificate chained to it is trusted by all major
internet browsers and operating systems.

## How is a Trust Chain Verified?

The client or browser inherently knows the Public-Keys of a handful of trusted
CAs and uses these keys to verify the server’s SSL certificate. The client
repeats the verification process recursively with each certificate in the Trust
Chain until tracing it back to the beginning, the root CA.

# What does an SSL Certificate do?

In unsecured HTTP connections, hackers can easily intercept messages between
client and server and read them in plain text. Encrypted connections scramble
communication until the client can decrypt it with the other session key.

When installed on a web server, SSL certificates use a public/private key pair
system to initiate the HTTPS protocol and enable secured connections for users
and clients to connect.

# For the Internet: What do SSL certificates do for websites?

When a signed SSL certificate secures a website, it proves that the
organization has verified and authenticated its identity with the trusted third
party; since the browser trusts the CA, the browser now trusts that
organization’s identity too.

Establishing a connection with a server with a certificate signed by a trusted
CA takes place without additional difficulties for the user. When an internet
user visits an SSL-secured website, they are more willing to submit their
contact information or shop with their credit card.

# For Intranets: What do SSL certificates do for applications in an enterprise environment?

The most common use cases for Enterprise SSL certificates include:

* Network Access controls
* Virtual Private Networks (VPN)
* Single sign-on
* Internet of Things(IoT)

If properly configured, all these applications run atop of SSL protocol. We’ll
take a closer look at these examples in the following section:

## Network Access

Employees who connect wireless devices to the corporate network have a need for
ease of access, while at the same time, the network must prevent unauthorized
access to corporate resources. Employees may use SSL certificates to access and
encrypt files from their devices, corporate servers, or even cloud servers for
approved individuals.

Avoid the need to remember/reset long, difficult to remember passwords that
change every 90 days by replacing it with a digital identity. Place a digital
identity into the Windows or Mac desktop, server, or WiFi access points, so
only authorized devices can connect to your corporate network.

## Single Sign-On

Today’s enterprise employees have access to a wide variety of Identity service
or federation products. Enterprises often use a Web Single Sign-on product to
access all its resources in the corporate portal or cloud services.

## Internet of Things

A digital identity can be installed in your IoT device and the user’s device or
application to ensure that only trusted IoT devices could connect to your
network. The IoT device takes instructions from or sends data to authorized
applications, and users possess a digital identity.

## SSL VPN

It is a virtual private network (VPN) created using the Secure Sockets Layer
(SSL) IT departments can scale both the solution and its required
infrastructure services. SSL VPN enables granular control over managed
application access to enterprise web applications. 

Related:
```
* https://www.keyfactor.com/blog/what-is-ssl-certificate/
```
