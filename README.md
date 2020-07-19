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


