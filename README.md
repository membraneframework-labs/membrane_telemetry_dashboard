# Membrane Telemetry Grafana

This repository provides introduction to integrating `membrane_timescaledb_reporter` and `membrane_pipeline_diagram_extractor` repositories with Grafana to create dashboards for monitoring your pipeline behaviour.

**Both repositories rely on schema created by** `membrane_timescaledb_reporter`.


`docker-compose.yml` contains fully functioning setup for local monitoring.

## Run
Due to some network problems when using Grafana with datasource from same docker's network one must provide `HOST_IP=<your machine ip>` to enable Grafana to connect with TimescaleDB.

```bash
# example to run given setup.
HOST_IP=192.168.38.64 docker-compose up
```

To access dashboard go to http://localhost:3000.

# Grafana Introduction
Grafana is a web browser tool that you will be able to access after running e.g. with help of docker.
Default credentials for a fresh Grafana instance are, both for username and passowrd: `admin`.

## Grafana Dashboard

Grafana's dashboard is a basic building block that will contain all your panels necessary for monitoring your pipeline.
You have to start by creating one and then adding new pannels that will be used for displaying graphs and and other types of visualization e.g. diagrams.

Grafana's power comes from an ability to easily share a dashboard layout between users. Dashboard can be imported to Grafana with a simple *.json* file.
We provide a simple dashboard consisting of 3 panels:
 - 2 separate graph panels plotting `Membrane.InputBuffer`'s internal buffer size inside of functions `take_and_demand/4` and `store/3`
 - pipeline diagram using Grafana's `Diagram` plugin
Provided `docker-compose`'s grafana setup is already equiped with mentioned dashboard.

To import custom dashboard navigate to Grafana's page and then to `Create > Import > Upload JSON file`, select your dashboard's file and click *Import*.

**Important**
Provided dashboard is available via Grafana's provisioning. It is set up to allow ui updates but remember that all your changes will be lost after `docker-compose` rebuild.
You might consider saving your modified dashboard somewhere in **.json** format and import it when needed or, if you find it useful, commit it to this repository under `grafana/provisioning/dashoards/` directory.

## Grafana's Data Source
A data source is used by Grafana to fetch necessary data requested by panels inside your dashboard.

Provided `docker-compose` setup comes with `TimescaleDB` instance which is added as default PostgreSQL source inside Grafana instance.


### Manual data source setup
Follow `Configuration > Data Sources > Add data source` and search for `PostgreSQL` and click *Select*.
Then you will need to provide connection information and enable `TimescaleDB` option in `PostgreSQL details` section.
You might need either to set current data source as default (at the very top of settings, right to the name) or change data source location inside of given dashboard's panels as those panels use a default one. 

## Creating new visualizations
You might want to create new panels that will visualize other aspects of your pipeline. To do this, you will need to provide them with previously created data source and create
proper SQL queries (provided that you still use data saved by `membrane_timescale_reporter`) that will fetch necessary data. 
You can inspect example's panels to see how to write such queries (they are very basic). You might also consider visiting TimescaleDB's and Grafana's documentations.

[TimescaleDB documentation](https://docs.timescale.com/latest/tutorials/tutorial-grafana-dashboards)

[Grafana documentation](https://grafana.com/docs/grafana/latest/panels/queries)

## Warnings
 - Be careful when querying large time ranges as some metrics might be reported thousands times per second and querying for example last 6h at once might crash your database instance or Grafana's dashboard.
 - Dashboard will not be fully funcitoning until first usage of `membrane_timescaledb_reporter` as fresh TimescaleDB instance does not have migrated tables.





