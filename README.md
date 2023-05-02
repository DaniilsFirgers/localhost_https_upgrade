# Upgrade http to https in local development

Sometimes there could be a need to upgrade your local development application to **behave like an HTTPS site**, so that it competely emulates your production version.

---

In order to do so you have to have a TLS/SSL certificate. However browsers will not accept justy any certificate - the certificate has to be signed by a trusted authority, called certificate authority (CA). You can check a list of trusted authorities in your browser by doing the following:

- Google Chrome

1. Click on the dots next to your account icon in the upper right corner and go to settings
2. Then navigate to privacy and security, and from there go ot security
3. Scroll to the bottom and go to 'Manage certificates'
4. Choose authorities tab

- Mozilla

1. Click on settings icon and go to 'Manage more settings'
2. Select Privacy & Security and scroll until Security part
3. Click on view certificates
4. Select authorities

Certified authorities distribute their certificates to computers worlwide via intermediaries that distribute certificates securely and reliably.

---

We can go through the same procedure and act as a CA as weel as issue a certificate for our domain name. Note - **Do not use self-signed sertificates in production!** One way is to use mkcert [MkCert](https://github.com/FiloSottile/mkcert). But in this guide wel will do it ourselves.

## Certificate Authority (CA)

1. Firstly we will generate a self-signed root certificate authority called localCA using OpenSSL:

```
openssl req -x509 -nodes -new -sha256 -days 1024 -newkey rsa:2048 -keyout localCA.key -out localCA.pem -subj "/C=US/CN=local-Root-CA"
```

You can change -days flag, which specifies the number of days fo which the certificate will be valid, -keyout flag that specifies the filename for the private key, -out flag that specifies the filename of the self-signed certificate file as well as -subj flag.

2. Next we convert the self-signed certificate from PEM to CRT format:

```
openssl x509 -outform pem -in localCA.pem -out localCA.crt
```

## Domain name certificate

In this example we will create the certirficate for one domain name. Let's say it is `example.local.net`. A general rule of thumb is to avoid the followinf domain names for local development: .dev , .local , .localhost, .test. Those can be reserved by Google Chrome and lead to some weird unexpected behaviour.

1. After you chose the domain name, you should go to `/etc/hosts` and add your custom domain name after localhost to the list like this:

```
127.0.0.1 example.local.net
```

**Do not delete the starting file content!**

2. Create a domains.ext file that include your local domain:

```
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1 = localhost
DNS.2 = example.local.net
```
