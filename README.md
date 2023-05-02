# Upgrade http to htpps in local development

Sometimes there could be a need to upgrade your local development application to **behave like an HTTPS site**, so that it competely emulates your production version.

---

In order to do so you have to have a TLS/SSL certificate. However browsers will not accept justy any certificate - the certificate has to be signed by a trusted authority, called certificate authority (CA). You can check a list of trusted authorities in your browser by doing the following:

- Google Chrome

1. Click on the dots next to your account icon in the upper right corner and go to settings
2. Then navigate to privacy and security, and from there go ot security
3. Scroll to the bottom and go to 'Manage certificates'
4. Choose authorities tab

- Mozilla
