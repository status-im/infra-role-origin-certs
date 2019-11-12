# Descirption

This role installs the certificate and key pair from CloudFlare which is called an __Origin__ certificate and is issued by CloudFlare CA to facilitate an SSL Proxy setup which allows the site to authenticate with the `*.status.im` wildcard certificate from CloudFlare without having it on the host.

>WARNING: The origin certificate alone is not enough to facilitate a valid SSL setup.

Details: https://blog.cloudflare.com/cloudflare-ca-encryption-origin/

# Configuration

```yaml
origin_certs:
  - domain: 'status.im'
    crt: '-----BEGIN CERTIFICATE-----\nABC...'
    key: '-----BEGIN PRIVATE KEY-----\n321...'
    default: true

  - domain: 'example.org'
    crt: '-----BEGIN CERTIFICATE-----\nXYZ...'
    key: '-----BEGIN PRIVATE KEY-----\n123...'
```

# Usage

The certificates end up under `/certs/${domain}` like so:

```
/certs/status.im/origin.crt
/certs/status.im/origin.key
/certs/example.org/origin.crt
/certs/example.org/origin.key
/certs/origin.crt -> /certs/status.im/origin.crt
/certs/origin.key -> /certs/status.im/origin.key
```
With the default cert being symlinked under `/certs/origin.{crt,key}` as a workarounf for old setup.

These certificates are used by services like Nginx or Grafana for the purpose of verifying their identity for CloudFlare proxy servers.
