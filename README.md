## ThousandEyes OTel | Zero Touch Testing Environment (ZTTE)

###  Deploy OTel Collector and other Visualization Services to stream OTel data from ThousandEyes

### Micro Service List
- Grafana (Data Visualization Tool)
- Prometheus (Time Series Database)
- OTel Collector (OpenTelemetry Collector)
- ngrok (Public Reverse Proxy)

### Prerequisites
- Docker Engine running on MacOS, Windows or Linux (https://docs.docker.com/engine/install)

### Create and configure your ngrok account (free)
1. This is required to create an public HTTPS endpoint for OTel Streaming from ThousandEyes (https://dashboard.ngrok.com/signup)
2. Go to "https://dashboard.ngrok.com/get-started/your-authtoken" page and reveal your token
3. Go to "https://dashboard.ngrok.com/domains" page and create a New Domain - This will ensure that your domain will be static between ngrok container restarts

### Prepare your environment
1. Clone Git Repository
```
git clone https://github.com/cyilmaze/thousandeyes-otel-zerotouch.git
```

2. Go to thousandeyes-otel-zerotouch folder and update NGROK environment variables in .env file
```
NGROK_AUTHTOKEN="<your auth token>"
NGROK_DOMAIN="<your domain>"
```

3. Run docker compose to deploy micro-services
```
docker compose up -d
```

3. Configure ThousandEyes OpenTelemetery Integration on ThousandEyes Portal
Important Config Options:
- Target: gRPC
- Data Model Version: v1
- OpenTelemetry Signal: Metric
```
https://docs.thousandeyes.com/product-documentation/integration-guides/custom-built-integrations/opentelemetry/configure-opentelemetry-streams/ui
```

### Verification Steps
1. Go to https://dashboard.ngrok.com/endpoints and confirm your endpoint is UP
2. Go to https://dashboard.ngrok.com and select Traffic Inspector from the left navigation. Ensure ThousandEyes Otel Stream traffic is reaching to ngrok. If not, wait for 10m and check your Integration on ThousandEyes.
3. Go to http://localhost:4040/inspect/http to confirm traffic is reaching to your ngrok proxy container which is running on your machine.
4. Go to your Terminal and check the otel-collector logs.
```
docker ps
docker logs thousandeyes-otel-zerotouch-otel-collector-1
```
Sample expected log line:
```
2025-02-25T12:56:33.573Z        debug   prometheusexporter@v0.120.1/collector.go:427    metric served: Desc{fqName: "net_metrics_loss_percentage", help: "Network metrics packet loss", constLabels: {}, variableLabels: {test_domain,test_type,agent_id,server_ip,server_name,test_id,datapoint_id,datapoint_created,datapoint_received,round_id,test_name,agent_name,agent_location,permalink,integration_id,account_id}}    {"otelcol.component.id": "prometheus", "otelcol.component.kind": "Exporter", "otelcol.signal": "metrics"}
```
5. Go to Prometheus on http://localhost:9090/targets and ensure targets are UP
6. Go to Grafana on http://localhost:3000/explore and select "Metric" and filter "net_metrics". (default credentials admin/admin)
