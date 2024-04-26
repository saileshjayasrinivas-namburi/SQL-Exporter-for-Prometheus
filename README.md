
# SQL Exporter for Prometheus 

## SQL Exporter is a configuration-driven exporter that exposes metrics gathered from SSMS, for use by the Prometheus monitoring system.

 

## Setup Instructions 

1. [Download SQL Exporter file](https://drive.google.com/file/d/1NNk3zKwIe5482gmkj1t2a5utnyvAZuAT/view)

2. [Download NSSM](https://nssm.cc/download)

3. Move both Folders to your C-Drive.

## Configuration 

1. Extract the SQL_EXPORTER folder

2. Open sql_exporter.yml file and edit data_source_name

```
data_source_name: 'sqlserver://username:password@Sqlmachine_IP:1433'
```

3. Replace **username**, **password**, and **Sqlmachine_IP** with the appropriate values. Remove /SQLEXPRESS from `data_source_name` if not required.


## Firewall Configuration

Add inbound rules in the SQL machine's Firewall settings for port 9399.

### Install NSSM:

Install NSSM (nssm 2.24 (2014-08-31)).

## Installation

1. Open Command Prompt in admin mode.

2. Navigate to the nssm2.24/win64 folder using the cd command.

```
nssm.exe install SQL_Exporter <path_to_sql_exporter>

```

3. After running the above command, open Task Manager and check whether SQL_Exporter is running.

## Configuration for Prometheus

[Download prometheus.yaml (values.yaml )](https://github.com/prometheus-community/helm-charts/blob/main/charts/kube-prometheus-stack/values.yaml) 


Update prometheus.yml:

1. Open prometheus.yml file in a text editor.
2. Locate scrape_configs section.
3. ADD below in scrape_configs
 
``` 
 - job_name: sql_exporter
  scheme: http
  static_configs:
    - targets: ['sql_machine_IP:9399'] # Example: ['10.0.0.0:9399']

```

## Apply Prometheus Configuration

```
helm repo add Prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install  --values Prometheus.yml prometheus prometheus-community/prometheus --namespace <namespace>

```

