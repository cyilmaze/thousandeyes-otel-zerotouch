[![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/cyilmaze/thousandeyes-otel-zerotouch)

# ThousandEyes OTel | Zero Touch Testing Environment (ZTTE)

**Deploy OTel Collector and other Visualization Services to stream OTel data from ThousandEyes**

## List of microservices
- Grafana (Data Visualization Tool)
- Prometheus (Time Series Database)
- OTel Collector (OpenTelemetry Collector)
- ngrok (Public Reverse Proxy)

## Prerequisites
- Docker Engine running on MacOS, Windows or Linux. Install [here](https://docs.docker.com/engine/install)

## Create and configure your ngrok account (free)
1. Sign up for an account at [ngrok](https://ngrok.com/signup). This is required to create an public HTTPS endpoint for OTel Streaming from ThousandEyes.
2. Retrieve your authentication token from the [ngrok Authtoken page](https://dashboard.ngrok.com/get-started/your-authtoken).
3. Navigate to the [ngrok Domains page](https://dashboard.ngrok.com/domains) and create a new domain. This ensures your domain remains static even after ngrok container restarts.

## Prepare your environment
1. Open Terminal and navigate to the Desktop or any folder where you want to clone the repository from GitHub.
```
git clone https://github.com/cyilmaze/thousandeyes-otel-zerotouch.git
```

2. Go to thousandeyes-otel-zerotouch folder, open .env file (it is a hidden file) and update NGROK environment variables.
```
NGROK_AUTHTOKEN="<your ngrok token>"
NGROK_DOMAIN="<your ngrok domain>"
```

3. Run docker compose to deploy micro-services
```
docker compose up -d
```

3. Configure ThousandEyes OpenTelemetery Integration on ThousandEyes Portal
- **Name:** My TE OTel Collector
- **Target:** gRPC https://***your ngrok domain***
- **Preset configurations:** Keep it as-is
- **Auth Type:** Keep it as-is
- **Customer Headers:** Not required
- **Data Model Version:** v1
- **OpenTelemetry Signal:** Metric
- **Test Data Configuration:** Select tests or tags that you want to export stream data
```
https://docs.thousandeyes.com/product-documentation/integration-guides/custom-built-integrations/opentelemetry/configure-opentelemetry-streams/ui
```

## Verification Steps
1. Confirm your [ngrok endpoint](https://dashboard.ngrok.com/endpoints) is UP
2. Select Traffic Inspector from the left navigation at [ngrok dashboard](https://dashboard.ngrok.com). Ensure ThousandEyes Otel Stream traffic is reaching to ngrok. If not, wait for 10m and check your Integration on ThousandEyes.
3. Confirm your [ngrok proxy container](http://localhost:4040/status) is Online.
4. Go to your Terminal and check the otel-collector logs.
```
docker ps
docker logs thousandeyes-otel-zerotouch-otel-collector-1
```
Sample expected log line:
```
2025-02-25T12:56:33.573Z        debug   prometheusexporter@v0.120.1/collector.go:427    metric served: Desc{fqName: "net_metrics_loss_percentage", help: "Network metrics packet loss", constLabels: {}, variableLabels: {test_domain,test_type,agent_id,server_ip,server_name,test_id,datapoint_id,datapoint_created,datapoint_received,round_id,test_name,agent_name,agent_location,permalink,integration_id,account_id}}    {"otelcol.component.id": "prometheus", "otelcol.component.kind": "Exporter", "otelcol.signal": "metrics"}
```
5. Ensure [Prometheus Targets](http://localhost:9090/targets) are UP
6. Visit [Grafana Dashboards](http://localhost:3000/dashboards), select "ThousadEyes Dashboard". (default credentials admin/admin)

## Troubleshooting Steps
1. If your ngrok endpoint doesn't receive any traffic, ensure your ThousandEyes Integration is pointing to correct domain with HTTPS scheme
2. If you ngrok container is not Online, ensure container is running and your ngrok token is correct
3. If you don't see data in Grafana dashboard, wait 10 after the initial deployment. Also, ensure that Prometheus Targets are UP.

## Credits
- [Aditya Chellam](https://github.com/adchella-te)
- [ Antonio Jimenez ](https://github.com/antonjim-te)
