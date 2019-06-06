# Guide to generate self-signed certificates

Self-signed server and client-side SSL

1) Create a Certificate Authority root (which represents the server)

Generate serverCA.key and serverCA.crt certificate using:

```openssl genrsa -out serverCA.key 4096```

```openssl req -x509 -new -nodes -key serverCA.key -sha256 -days 1024 -out serverCA.pem```

Note: If you need to provide a passphrase then add "-des3" flag on above command line. 

For the server certificate you'll be requested to provide Organization & Common Name. The information provided can be any human identifiers for this server CA.

2) Create the Client Key and CSR

Organization & Common Name should be the client specific indentifiers 

```openssl genrsa -out clientCert.key 4096```
```openssl req -new -key clientCert.key -out clientCert.csr```

Generate the self-signed client certificate

```openssl x509 -req -in clientCert.csr -CA serverCA.pem -CAkey serverCA.key -CAcreateserial -out clientCert.pem -days 500 -sha256```

Convert Client Key to PKCS

Doing so, the client certificate may be installed in most browsers.

```openssl pkcs12 -export -clcerts -in clientCert.pem -inkey client.key -out clientCert.p12```

Note: clientCert.pem can also be created using
     
```cat clientCert.key clientCert.csr > clientCert.pem```

Procedure to generate server certificate with passphrase and convert to .pem

Step 1: Generate a Private Key 

```openssl genrsa -des3 -out server.key 1024```

or

```openssl genrsa -des3 -out server.key 2048```

Step 2: Generate a CSR (Certificate Signing Request) 

```openssl req -new -key server.key -sha256  -out server.csr```

Step 3: Generating a Self-Signed Certificate 

```openssl x509 -req -days 365 -in server.csr -signkey server.key -sha256 -out server.crt```

Step 4: Convert the CRT to PEM format

```openssl x509 -in server.crt -out server.pem -outform PEM```
