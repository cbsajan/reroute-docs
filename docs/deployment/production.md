# Production Deployment

Deploy REROUTE applications to production.

## Production Configuration

```python
from reroute import Config

class ProductionConfig(Config):
    DEBUG = False
    PORT = 8000
    HOST = "0.0.0.0"

    # CORS
    ENABLE_CORS = True
    CORS_ALLOW_ORIGINS = ["https://yourdomain.com"]

    # Security Features (v0.2.0+)
    MAX_REQUEST_SIZE = 16 * 1024 * 1024  # 16MB limit
    HEALTH_CHECK_AUTHENTICATED = True  # Protect detailed health endpoint

    # Logging
    VERBOSE_LOGGING = True
    LOG_LEVEL = "WARNING"
```

## Environment Variables

`.env`:
```bash
# Basic Configuration
REROUTE_PORT=8000
REROUTE_HOST=0.0.0.0
REROUTE_DEBUG=False
REROUTE_CORS_ORIGINS=https://yourdomain.com

# Security Configuration (v0.2.0+)
REROUTE_SECRET_KEY=your-256-bit-secret-key-here
REROUTE_MAX_REQUEST_SIZE=16777216  # 16MB
REROUTE_HEALTH_CHECK_AUTHENTICATED=true

# Database (if using)
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
```

Load in your app:
```python
from dotenv import load_dotenv
load_dotenv()
```

## Using Uvicorn (FastAPI)

```bash
# Production server
uvicorn main:app \
    --host 0.0.0.0 \
    --port 8000 \
    --workers 4 \
    --no-access-log
```

## Using Gunicorn

```bash
# With Uvicorn workers
gunicorn main:app \
    -w 4 \
    -k uvicorn.workers.UvicornWorker \
    -b 0.0.0.0:8000
```

## Nginx Configuration

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## Systemd Service

`/etc/systemd/system/reroute-app.service`:
```ini
[Unit]
Description=REROUTE Application
After=network.target

[Service]
User=www-data
WorkingDirectory=/var/www/app
ExecStart=/usr/bin/uvicorn main:app --host 0.0.0.0 --port 8000
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start:
```bash
sudo systemctl enable reroute-app
sudo systemctl start reroute-app
```

## Security Checklist (v0.2.0+)

### ✅ Critical Security Settings
- [ ] Set `DEBUG = False`
- [ ] Use HTTPS with valid certificates
- [ ] Configure proper CORS origins (no wildcards)
- [ ] Set strong `REROUTE_SECRET_KEY` (256-bit minimum)
- [ ] Enable request size limits (`MAX_REQUEST_SIZE`)
- [ ] Protect health endpoints (`HEALTH_CHECK_AUTHENTICATED = True`)

### 🛡️ Application Security
- [ ] Set rate limiting on all endpoints
- [ ] Implement authentication with `@requires` decorator
- [ ] Validate all inputs with Pydantic models
- [ ] Use parameterized queries (built-in with REROUTE models)
- [ ] Enable security logging (automatic with decorators)

### 🔒 Infrastructure Security
- [ ] Use environment variables for secrets
- [ ] Set up monitoring and alerting
- [ ] Configure firewall rules
- [ ] Use reverse proxy (Nginx/Apache) with security headers
- [ ] Enable WAF if available
- [ ] Regular security updates

### 📊 Monitoring & Observability
- [ ] Enable structured security logging
- [ ] Set up alerts for security events
- [ ] Monitor rate limit violations
- [ ] Track authentication failures
- [ ] Regular security scans

## Monitoring

Use tools like:
- Prometheus + Grafana
- Sentry for error tracking
- Datadog
- New Relic

## Performance

- Use multiple workers
- Enable caching
- Use CDN for static files
- Optimize database queries
- Enable compression
