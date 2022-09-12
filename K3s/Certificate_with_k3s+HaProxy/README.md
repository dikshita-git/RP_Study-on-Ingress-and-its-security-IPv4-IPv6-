Here, we are setting up a TLS using HaProxy Ingress Controller meaning all the exteranl traffic passes through this ingress controller which also implies that at thsi very exact point, the TLS can be set up to secure th etraffic.
By doing so, all the services running inside our k3s cluster inherit the TLS. Additionally, hereby the focus is also on setting up certificates using Lets Encrypt.

I used Helm in this context and installed it following <a href="https://helm.sh/docs/intro/install/#from-script">Here</a>.

Below are teh steps followed and a descriptive detail herewith is also available:

## STEPS:

### 1. Add the HaProxy helm repository and update it

Commands:

```
helm repo add haproxytech https://haproxytech.github.io/helm-charts
```

```
helm repo update
```
