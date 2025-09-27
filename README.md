# Uptime Kuma with Caddy Reverse Proxy

This Docker Compose setup runs Uptime Kuma with Caddy as a reverse proxy for automatic SSL certificates.

## Prerequisites

- Docker and Docker Compose installed
- A domain name pointing to your server

## Setup

1. Clone or download this repository
2. Edit the `Caddyfile` and replace `your-domain.com` with your actual domain
3. (Optional) Copy `env-example.txt` to `.env` and modify configuration as needed
4. Run the containers:

```bash
docker-compose up -d
```

The `docker-compose.override.yml` file automatically loads environment variables from the `.env` file.

## Configuration

### Domain Configuration

Edit the `Caddyfile` and replace `your-domain.com` with your actual domain name.

### Environment Variables

You can create a `.env` file in the same directory as `docker-compose.yml` to override default settings:

```env
# Uptime Kuma port (default: 3001)
UPTIME_KUMA_PORT=3001

# Disable frame same-origin (set to 1 to disable)
UPTIME_KUMA_DISABLE_FRAME_SAMEORIGIN=0
```

### SSL Certificates

Caddy will automatically obtain and renew SSL certificates from Let's Encrypt. Certificates are stored in the `caddy_data` volume.

## Usage

- Access Uptime Kuma at `https://your-domain.com`
- The setup includes automatic HTTPS redirection
- WebSocket connections are properly proxied for real-time updates

### Local Testing

For local development and testing without SSL:

```bash
docker-compose -f docker-compose.test.yml up -d
```

Access at `http://localhost:8080`

This setup exposes Uptime Kuma directly on port 3001 and Caddy on port 8080 for testing.

## Volumes

- `./data`: Uptime Kuma application data
- `caddy_data`: SSL certificates and ACME data
- `caddy_config`: Caddy configuration files

## Security Features

- Automatic HTTPS with Let's Encrypt
- Security headers (HSTS, XSS protection, etc.)
- Rate limiting
- Proper WebSocket proxying

## Backup

To backup your data:

```bash
# Stop containers
docker-compose down

# Backup data directory
tar -czf uptime-kuma-backup-$(date +%Y%m%d).tar.gz data/

# Start containers again
docker-compose up -d
```

## Troubleshooting

### Check logs

```bash
# Uptime Kuma logs
docker-compose logs uptime-kuma

# Caddy logs
docker-compose logs caddy
```

### Common issues

1. **Port 80/443 already in use**: Make sure no other web servers are running
2. **Domain not resolving**: Ensure your domain DNS points to your server
3. **SSL certificate issues**: Check that ports 80 and 443 are accessible from the internet

## Updating

To update the containers:

```bash
docker-compose pull
docker-compose up -d
```
