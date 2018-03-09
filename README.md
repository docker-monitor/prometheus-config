# prometheus-config
Dockerization prometheus and grafana

## Description

Here will give you a setup guideline about prometheus server, consul, grafana and alertmanager.

## 1. initialization

First we  need initialization a work directory, and then you can drict copy this git monitor folder to you workdir.

## 2. start alertmanager(optional)
> if you don`t need alert component, you can skip this step and you need comment **rule_files** at next step.
* update config.yml under monitor/alertmanager/config/
    * smtp_smarthost (update to your smtp server)
    * update **name** and **to** to your value.
    ```yaml
    receivers:
    - name: '${name}'
    email_configs:
    - to: '${your_email}@address'
        require_tls: false
    ```
* startup alertmanager
    ```bash
    cd monitor/alertmanager
    docker-compose up -d
    ```
* check starting status
    ```bash
    docker ps
    ```


## 3. start prometheus server

* update below args in monitor/docker-compose.yml
    * alertmanager.url
        > alertmanager.url need update to your alertmanger server.
* update below args in monitor/prometheus/prom-conf/prometheus.yml
    * targets (It`s your prometheus server url.You can reference service prometheus in monitor/docker-compose.yml)
    * consul_sd_configs
        ```yaml
        consul_sd_configs:
            - server:   'consul_server:13002'
        ```
    * rule_files
        > It is define your alter rules file path, you can find a example at  monitor/prometheus/prom-conf/rules/

Other args you can use default value, if you need update, you can reference prometheus official doc.
* startup
    ```bash
    cd monitor
    docker-compose up -d (this step may need long time for pull docker image)
    docker ps
    ```
    if you run **docker ps**, one more three container running(container name like:monitor_consul_1, monitor_prometheus_1, monitor_grafana_1), Congratulations. 

