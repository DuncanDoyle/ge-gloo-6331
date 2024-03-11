# Gloo-6331 Reproducer

## Installation

Add Gloo EE Helm repo:
```
helm repo add glooe https://storage.googleapis.com/gloo-ee-helm
```

Export your Gloo Edge License Key to an environment variable:
```
export GLOO_EDGE_LICENSE_KEY={your license key}
```

Install Gloo Edge:
```
cd install
./install-gloo-edge-enterprise-with-helm.sh
```

> NOTE
> The Gloo Edge version that will be installed is set in a variable at the top of the `install/install-gloo-edge-enterprise-with-helm.sh` installation script.

## Setup the environment

Creata DNS record in duckdns.org with the name `gloo-test.duckdns.org` that points to the ip-address of httpbin.org.

Next, run the `install/setup.sh` script to setup the environment:

- Deploy the Upstream
- Deploy the VirtualServices

```
./setup.sh
```

## Call HTTPBin

```
curl -v http://api.example.com/httpbin/get
```

This will route the traffic to httpbin.org via our static Upstream, which uses the DNS name `gloo-test.duckdns.org` to route the traffic to httpbin.org.

Remove the `gloo-test.duckdns.org` DNS name. After a while (usually less than a minute), Gloo Edge/Envoy refreshes the DNS. When you now call HTTPBin:

```
curl -v http://api.example.com/httpbin/get
```

You will get a 503 - Service Unavailable response and the message "no healthy upsteam" in the response body.


## Conclusion
Can't reproduce.