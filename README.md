# Simplificator's Caddy Reverse Proxy

[![Build](https://github.com/simplificator/caddy-reverse-proxy/actions/workflows/build.yml/badge.svg)](https://github.com/simplificator/caddy-reverse-proxy/actions/workflows/build.yml)

This repository provides a modified Caddy to pass the active domains as environment variables. Certificates are issued automatically.

The following environment variables are required:

* `ALTERNATIVE_DOMAINS`: A comma-separated list of additional domains for which Caddy should get certificates. Requests to these domains redirect to `MAIN_DOMAIN`.
* `MAIN_DOMAIN`: The main domain for your site.
* `UPSTREAM_URL`: The service where Caddy sends the incoming requests.

We recommend mounting `/data` out of the container as Caddy saves the retrieved certificates and additional metadata to this directory.

Example:

```yaml
version: "3.9"
services:
  app:
    image: "user/myapp"

  caddy:
    image: simplificator/caddy-reverse-proxy:1
    volumes:
      - /data/caddy:/data
    environment:
      ALTERNATIVE_DOMAINS: "another.domain,second.domain"
      MAIN_DOMAIN: "main.domain"
      UPSTREAM_URL: "app:3000"
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
```

## License

Our configuration is released under the MIT license. Caddy is a registered trademark of Stack Holdings GmbH.
