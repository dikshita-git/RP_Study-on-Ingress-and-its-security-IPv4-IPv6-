As the Secure Socket Layer ormost commonly SSSL has become ooutdated, thus here comes in the TLS which is basically a Transport Layer Security Protocol. This protocol adds a layer of security on top of the HTTP connection whicch means it wraps around the HTTP connection and thus the data which gets excahnged between the server ansd client is secured.
First and foremost, if the server has to have a security layer then it means it needs to have certificate from a trusted and valid authority. If it is from a non-trusted authority, it may send up a risk page ("Your connection is not private"). The data which was earlier sent through HTTP was using a plain-text form and once we use HTTPS, it is a ciphertext.

in this part, we are trying to get a valid certificate fro our sample application which runs in our k3s cluster. We install:

1. Nginx as ingress controller

2. Cert-manager as certificate manager

  - We need cert-manager so taht we can communicate with our certificate provider. In short, we will configure cert-manager to speak to Lets encrypt

Results expected:
----------------------

We obtain valid certificate for our domain by using the non-profit authority called Lets Encrypt. Lets Encrypt offers different ways to obtain a certificate. In order to obtain a valid certificate we have to prove that the domain,server for which we need teh certificate is owned by us.
The tool used to do this is cert-manager.

### Steps followed:

1. Disable traefik with :

```
export INSTALL_K3S_EXEC="server --no-deploy traefik"
```

