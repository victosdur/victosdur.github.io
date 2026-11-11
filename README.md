# Víctor Toscano Durán — Personal website

A responsive, static-first personal website served by Nginx in Docker. No build step or JavaScript framework is required.

## Project structure

```text
victor-portfolio/
├── assets/
│   └── cv.pdf
├── index.html
├── styles.css
├── script.js
├── Dockerfile
├── docker-compose.yml
├── nginx.conf
└── README.md
```

## Run locally with Docker

Requirements: Docker Engine and Docker Compose plugin.

```bash
docker compose up -d --build
```

Open <http://localhost:8080>.

To inspect logs:

```bash
docker compose logs -f portfolio
```

To stop:

```bash
docker compose down
```

## Deploy with a domain and HTTPS

Recommended production architecture:

```text
Internet
   |
   v
your-domain.com (DNS A/AAAA records)
   |
   v
Reverse proxy (Caddy / Traefik / Nginx Proxy Manager)
   |  TLS certificate + HTTPS
   v
portfolio container (private Docker network, port 80)
```

For a public server, do not expose the portfolio container directly on port 80 if a reverse proxy already manages HTTP/HTTPS. Remove the `ports` mapping from `docker-compose.yml`, attach the service to the reverse proxy's Docker network, and configure the proxy to forward to `portfolio:80`.

If you want a quick standalone test, the included compose file publishes port 8080. For production, point your domain DNS to your server's public IP and configure your reverse proxy to obtain and renew a Let's Encrypt certificate. Keep ports 80 and 443 open on the server firewall.

Example Caddy site block (adapt the domain and Docker networking to your setup):

```caddy
your-domain.com {
    reverse_proxy portfolio:80
}
```

The Caddy container and portfolio container must share a Docker network for the service name `portfolio` to resolve.

## Personalise before publishing

1. Replace `assets/cv.pdf` with the CV version you want to make public.
2. Review the social profile links in `index.html` (especially Google Scholar and LinkedIn).
3. Add project cards and links to public repositories or demos when ready.
4. Review publication metadata and DOI links against the final publisher records.
5. Consider adding a dedicated publications page or BibTeX download if your bibliography grows.

## Notes

- The site is static and does not collect visitor data.
- The contact CTA opens the visitor's email client; it is not a server-side contact form.
- Docker and deployment skills are presented as an engineering direction/toolkit, not as a claim of prior production infrastructure work.
- Google Fonts are loaded externally. For a fully self-hosted setup, download and serve the font files locally.
