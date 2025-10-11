# Docker SSHwifty

This project provides a Docker Compose setup for running [SSHwifty](https://github.com/niruix/sshwifty), a web-based SSH client that allows you to connect to SSH servers directly from your browser.

## Overview

SSHwifty is a powerful web SSH client that provides:
- Browser-based SSH connections
- No local client installation required
- Secure connection handling
- Modern web interface

## Prerequisites

- Docker
- Docker Compose
- An external Docker network named `proxy_frontend` (for reverse proxy integration)
- suitable for using in a [Portainer](https://portainer.io) setup

## Installation and Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/egoexpress/docker-sshwifty.git
   cd docker-sshwifty
   ```

2. Ensure the external network exists (create if needed):
   ```bash
   docker network create proxy_frontend
   ```

3. Start the service:
   ```bash
   docker-compose up -d
   ```

## Configuration

The default configuration uses:
- Image: `niruix/sshwifty:0.4.1-beta-release`
- Restart policy: `unless-stopped`
- External network: `proxy_frontend`

### Custom Configuration

You can override the default configuration by creating a `docker-compose.override.yml` file (which is ignored by git). This allows you to:
- Change ports
- Add environment variables
- Modify volume mounts
- Adjust network settings

Example override file:
```yaml
version: '3'
services:
  app:
    ports:
      - "8080:8182"
    environment:
      - CUSTOM_VAR=value
```

## Usage

Once running, SSHwifty will be accessible through your reverse proxy configuration. The exact URL depends on how you've configured your proxy to route to this service.

## Network Configuration

This setup uses an external network (`proxy_frontend`) to facilitate integration with a reverse proxy setup. This allows:
- Easy routing from reverse proxy containers
- Service discovery within the proxy network
- Isolation from other Docker networks

## Troubleshooting

### Service won't start
1. Ensure the `proxy_frontend` network exists:
   ```bash
   docker network ls | grep proxy_frontend
   ```
2. Check container logs:
   ```bash
   docker-compose logs app
   ```

### Can't access through reverse proxy
1. Verify the reverse proxy is configured to route to this container
2. Ensure both services are on the `proxy_frontend` network
3. Check network connectivity between containers

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project's license matches the upstream SSHwifty project. Please refer to the [SSHwifty repository](https://github.com/niruix/sshwifty) for licensing information.

## Support

For SSHwifty-specific issues, please refer to the [official SSHwifty repository](https://github.com/niruix/sshwifty).

For Docker setup issues, please create an issue in this repository.