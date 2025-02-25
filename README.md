## ThousandEyes OTel | Zero Touch Testing Environment (ZTTE)

### Micro Service List
- Grafana
- Prometheus
- OTel Collector
- ngrok
- Nginx (Only for debugging purposes)

### Prerequisites
- Docker Engine running on MacOS, Windows or Linux (https://docs.docker.com/engine/install)
- ngrok account - this is required to create an public HTTPS endpoint for OTel Streaming from ThousandEyes (https://dashboard.ngrok.com/signup)

1. Update your ngrok credentials
```
Update .env file using the parameters from ngrok
```

2. Run docker compose to deploy micro-services
```
docker compose up -d
```

3. Configure ThousandEyes OpenTelemetery Integration on ThousandEyes Portal