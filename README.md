# caddy-cloudflare

[![](https://img.shields.io/docker/cloud/build/technoguyfication/caddy-cloudflare)](https://hub.docker.com/r/technoguyfication/caddy-cloudflare)
[![](https://img.shields.io/docker/pulls/technoguyfication/caddy-cloudflare)](https://hub.docker.com/r/technoguyfication/caddy-cloudflare)

Caddy with integrated support for Cloudflare DNS-01 ACME verification challenges.

I'm using this image in production myself, but you may wish to fork it and deploy your own version rather than trust my image (I would recommend you do).

**Please see the official [Caddy Docker Image](https://hub.docker.com/_/caddy) for more detailed deployment instructions.**

## Images

Includes images for regular and alpine versions of Caddy. Each are rebuilt every Monday morning at 0300 UTC from the `:latest` and `:alpine` tags respectively. Visit this repository on [Docker Hub](https://hub.docker.com/r/technoguyfication/caddy-cloudflare) to pull images.

## Requirements
1. A Cloudflare account
2. All domains you want to use with Caddy MUST be on your Cloudflare account, for any domains not through Cloudflare you must fall back to another verification method using the `tls` block [here](https://caddyserver.com/docs/caddyfile/directives/tls).


## Notes

Caddy will use DNS-01 ACME verification to generate certificates for any domains you specify in your Caddyfile. You can also use wildcard domains (e.g. `*.example.com`) in your Caddyfile and certificates will be obtained for them. Substitute the `:latest` tag for `:alpine` to use a smaller base image with higher performance and less overhead.
## Instructions:

1. Obtain your Cloudflare API token by visiting your Cloudflare dashboard and creating a token with the following permissions:
	- Zone / Zone / Read

	- Zone / DNS / Edit

	The token does not need any more permissions than these for DNS-01 ACME verification.

2. Add this to your Caddyfile (or create one with this):
	```Caddyfile
	{
		acme_dns cloudflare {$CLOUDFLARE_API_TOKEN}
		email   {$ACME_EMAIL}
	}
	```

3. Start your Docker container using the following command (substituting your own token and email address):
	```
	docker run -it --name caddy \
		-p 80:80 \
		-p 443:443 \
		-v caddy_data:/data \
		-v caddy_config:/config \
		-v $PWD/Caddyfile:/etc/caddy/Caddyfile \
		-e ACME_EMAIL=me@example.com \
		-e CLOUDFLARE_API_TOKEN=123457890 \
		-e ACME_AGREE=true \
		technoguyfication/caddy-cloudflare:latest
	```

	Or for docker-compose:
	```yaml
    version: "3.7"

    services:
    caddy:
      image: technoguyfication/caddy-cloudflare:latest
      restart: unless-stopped
      environment:
      - ACME_EMAIL="me@example.com"
      - CLOUDFLARE_API_TOKEN=1234567890
      - ACME_AGREE=true
      ports:
      - "80:80"
      - "443:443"
      volumes:
      - caddy_data:/data
      - caddy_config:/config
      - $PWD/Caddyfile:/etc/caddy/Caddyfile

    volumes:
      caddy_data:
      caddy_config:
	```
