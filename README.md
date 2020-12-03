# proxysql-grafana-prometheus
A Dockerized Grafana / Prometheus environment for ProxySQL dashboards


- Ensure `docker` is installed and running as well as `docker-compose` (see https://docs.docker.com/get-docker/)
- In your ProxySQL instances make sure to enable the restapi and specify a free port by configuring the following ProxySQL Admin variables:
```
SET admin-restapi_enabled='true';
SET admin-restapi_port=6070;

LOAD ADMIN VARIABLES TO RUNTIME;
SAVE ADMIN VARIABLES TO DISK;
```
- Edit the `prometheus/prometheus.yml` and add your ProxySQL instances:
```
  - job_name: 'proxysql'
    scrape_interval: 5s

    static_configs:
         - targets: ['192.168.1.28:6070','192.168.1.28:6071']

```
- Run `docker-compose up`
- Once instances are up you can connect to `http://localhost:3000` (or `http://<ip of server>:3000`)
- The default credentials are `admin` / `foobar` (this can be edited in `grafana/config.monitoring` by specifying a different `GF_SECURITY_ADMIN_PASSWORD`)
- Dashboards are provisioned from the `grafana/provisioning/dashboards` directory (save JSON dashboards to be used as templates here):
```
$ ls -l grafana/provisioning/dashboards/
total 116
-rw-rw-r-- 1 root root 18541 Dec  2 23:59 ProxySQL-Fleet-Overview.json
-rw-rw-r-- 1 root root 92584 Nov 13 01:03 ProxySQL-Host-Statistics.json
-rw-r--r-- 1 root root   184 Oct  8 16:09 dashboard.yml
```
