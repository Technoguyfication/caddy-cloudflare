# caddy-cloudflare
Caddy with integrated support for Cloudflare DNS-01 ACME verification challenges.

I'm using this image in production myself, but you may wish to fork it and deploy your own version rather than trust my image (I would recommend you do).

**Please see the official [Caddy Docker Image](https://hub.docker.com/_/caddy) for deployment instructions.**

## Images

Includes images for regular and alpine versions of caddy. Each are rebuilt every Monday morning from the `:latest` and `:alpine` tags respectively.
Visit this repository on [Docker Hub](https://hub.docker.com/r/technoguyfication/caddy-cloudflare) to pull images.

## Notes

Sample for running this image using Cloudflare verifications:

```
docker run -it --name caddy \
	-p 80:80 \
	-p 443:443 \
	-v caddy_data:/data \
	-v caddy_config:/config \
	-v $PWD/Caddyfile:/etc/caddy/Caddyfile \
	-e CLOUDFLARE_EMAIL=me@example.com \
	-e CLOUDFLARE_API_TOKEN=12345 \
	-e ACME_AGREE=true \
	technoguyfication/caddy-cloudflare:latest
```

To obtain your Cloudflare API token, visit your Cloudflare dashboard and create a token with the following permissions:
- Zone / Zone / Read
- Zone / DNS / Edit

You don't need any more permissions than these for DNS-01 ACME verification.
