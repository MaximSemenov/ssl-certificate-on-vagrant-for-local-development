# ssl-certificate-on-vagrant-for-local-development
How to install SSL certificate on Vagrant machine for local development in Google Chrome to use HTTPS

1) You need to have the openssl command utility installed. Open terminal and check that out by typing 

`openssl version `

You will see something like **LibreSSL 2.6.5** if you have it installed

2) Generate private key by executing this in terminal

`openssl genrsa -out ./vagrant/example.com.key 2048`

- **./vagrant/** - directory I have my project in; 

- **example.com** - your local domain name that 

- notice **.key** extention - it is just a filename with *.key* extention

3) Now we have our key generated let's use the key to generate a certificate 

`openssl req -new -x509 -nodes -key ./vagrant/example.com.key -out ./vagrant/example.com.cert -days 3650 -subj /CN=example.com`

4) This step we need to find Apache congif. My Apache config for Vagrant is located here *example.com/vagrant/apache/apache.conf*

where **example.com** is my project directory

5) Find or if it does not exist this clock `<VirtualHost *:443> HERE IN BETWEEN VirtualHost TAGS SHOULD BE RULES FOR YOUR HTTPS </VirtualHost>`

```
<VirtualHost *:443>

    # your server name here:
    ServerName example.com
    
    # your path to the project here:
    DocumentRoot "/app/htdocs"

    # here you saying please turn:
    SSLEngine on
    
    # here is the path to your certificate:
    SSLCertificateFile /vagrant/example.com.cert
    
    # here is the path to your key:
    SSLCertificateKeyFile /vagrant/example.com.key

    # down here more config rules that are not related to SSL
    # ... etc

</VirtualHost>
```
6) Now Apache server knows what to do with **https** requests let's explain it how to seal with **http**. Add another *<VirtualHost>* block of code bu this time with port 80 (if you already have block **<VirtualHost *:80>** edit it)

```
<VirtualHost *:80>
        ServerName example.com
        DocumentRoot "/app/htdocs"
        Redirect permanent / https://example.com
</VirtualHost>
```
We just said to Apache if someone comes via **http** redirect them to *https://example.com*

