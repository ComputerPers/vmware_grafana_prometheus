# Monitoring vSphere using Grafana and Telegraf, Victoriametrics/Prometheus
A way to retrieve vCenter information and integrate it with Victoriametrics, to present it later with Grafana

> [!IMPORTANT]
> ### Requirements:
> - vCenter 6.5 or newer
> - Telegraf 1.0 or newer 
> - Prometheus / Victoriametrics 1.90
> - Grafana 11 or newer
> - DNS


# A сouple of Grafana dashboards.
## Hosts

![image](https://github.com/user-attachments/assets/55861345-307a-4c88-87bd-33dbfd946ad8)

## vSphere Overview

![image](https://github.com/user-attachments/assets/5a47f047-7e17-403a-8b55-633bf931f164)

![2025-03-03_13-02-14](https://github.com/user-attachments/assets/0d426cb6-89b8-4840-8b91-80d87fc656b8)

![image](https://github.com/user-attachments/assets/2a9df9da-a926-4b97-b93f-cbbbd5d913fe)


# Configuration
## vCenter

- Create ReadOnly user
- Configure [config.vpxd.stats.maxQueryMetrics](https://knowledge.broadcom.com/external/article?legacyId=2107096)

## Telegraf

- configure [Telegraf Input](https://github.com/influxdata/telegraf/blob/master/plugins/inputs/vsphere/README.md) 
```
[[inputs.vsphere]]
  interval = "60s"
  vcenters = [ "https://someaddress/sdk" ]
  username = "readonly@vsphere.local"
  password = "secret"

  insecure_skip_verify = true
  force_discover_on_init = true
```
- Add [Prometheus output](https://github.com/influxdata/telegraf/blob/master/plugins/outputs/prometheus_client/README.md) section to your telegraf.conf
```
  [[outputs.prometheus_client]]
  listen = ":9273"
```
- Add your Telegraf agent name to DNS (example: telegraf-vcenter.local.domain)
- Check metrics - http://telegraf-vcenter.local.domain:9273/metrics

## Victoriametrics / Prometheus

- Setup target for prometheus agents:
```
- targets:
  - 'telegraf-vcenter.local.domain:9273'

  labels:
    job: vcenters
```

  
## Grafana
- Add your Victoriametrics as [Prometheus datasource](https://docs.victoriametrics.com/#grafana-setup)
- Import Dashboards from this repository


Repository for the VMware, Grafana, Telegraf an InfluxDB integration - https://github.com/jorgedlcruz/vmware-grafana
