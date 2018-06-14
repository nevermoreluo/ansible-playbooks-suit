# Ansible-playbooks-suit
Ansible playbooks for myself  
Only support ubuntu 14.04 or above  
Do not forget modify you inventory file default `/etc/ansible/hosts`
Ex:
```ini
[node]

laxzl01 domain=laxzl01.expamle.io ansible_host=xxx.xxx.xxx.xxx
```

`Note: It will auto install python2, if not exists.`


## Road map
Just an ansible playbooks deploy base monitor for my all vps



## Roles

### common
An base common setup for all roles
- Modify ulimit
- Setup ntp
- Add keys for ssh login

### nodeExporter
VPS metrics data collector one of prometheus powerful plugins
default start nodeExporter on port 7999 :(
NodeExporter will install in /opt/prometheus_exporters
Will start by `service prometheus-node-exporter start`


### openresty
nginx with lua module
auto include ngx_lua_waf
and disable nginx



Also you can run it with docker,
```bash
docker run -d --name node -p 7999:9100 -v "/proc:/host/proc" -v "/sys:/host/sys" -v "/:/rootfs"  prom/node-exporter --path.procfs /host/proc --path.sysfs /host/proc --collector.filesystem.ignored-mount-points "^/(sys|proc|dev|host|etc)($|/)" 
```


## TODO
- [MonitorServer] run prometheus by docker
- [MonitorServer] run pushgateway by docker
- [MonitorServer] run alertmanager by docker
- [MonitorServer] run grafana by docker 

Based on following commands

```bash
apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common -y &&  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && apt-key fingerprint 0EBFCD88 && add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable" && apt update && apt-get install docker-ce -y
   
docker run --name pushgateway -d -p 9091:9091 prom/pushgateway
docker run --name prometheus -d -p 9090:9090 -v /data/prometheus:/prometheus-data  prom/prometheus --config.file=/prometheus-data/config.yml
docker run --name alertmanager -d -p 9093:9093 -v /data/prometheus:/prometheus-data  prom/alertmanager --config.file=/prometheus-data/alertmanager_config.yml
```


```test
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;
worker_rlimit_nofile 1048576;


# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 10240;
}
```

