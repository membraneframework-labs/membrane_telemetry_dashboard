# Membrane Telemetry Dashboard

This repository provides introduction to integrating `membrane_timescaledb_reporter` repository with `membrane_dashboard` to monitor your pipeline behaviour.

**Both repositories rely on schema created by** `membrane_timescaledb_reporter`. 

To explore database config and migrations pleases refer to [membrane_timescaledb_reporter](https://github.com/membraneframework/membrane_timescaledb_reporter) repository.

Please note that this setup by default uses `membrane_timescaledb_reporter` database name and other predefined variables like **username** and **password** which are default to reporter's default config. 
If you have to change it while using **reporter** package make sure you update `docker-compose.yml` file as well.


## Note
`docker-compose.yml` contains fully functioning setup for local monitoring.

## Run
```bash
docker-compose up
```

To access dashboard go to http://localhost:8000.

# Dashboard Overview
**Membrane Dashboard** is a web browser application that you will be able to access e.g. by running with help of docker.
It consists of:
  - time series charts 
  - graphs visualizing pipelines

You can specify time range of displayed data and accuracy of the charts.
Charts and graphs can be updated automatically every 5 seconds. This option is enabled by default. 
It can be disabled by clicking **Search** button and be enabled back with clicking **Last 5 min** or **Last 10 min** buttons. 

## Charts
Dashboard displays one chart for every `Membrane.Core.InputBuffer` inside function that reports current buffer's size.

You can zoom in by selecting time period on the chart. 
Be aware that automatic update will zoom out the chart, so it is convenient to disable real-time update when analyzing data.
Click twice on the chart to zoom out.

## Graphs
Dashboard shows also directed graphs visualizing pipelines' elements and links between them.

You can centre specific pipeline by clicking on its name in **Pipelines** list.

It is also possible to zoom and drag elements using mouse.

## Data Sources
Provided `docker-compose.yml` file sets up **Membrane Dashboard** and database (TimescaleDB). 
To fetch some data, use any Membrane application integrated with `membrane_timescaledb_reporter`.
Be aware that you have to specify the same database configuration in both Membrane application and `docker-compose.yml`.

## Warnings
 - Be careful when querying large time ranges with high accuracy as some metrics might be reported hundreds times per second and querying for example last 6h at once might crash your database instance or **Membrane Dashboard**.
 - After any change in number of different metrics stored in the database (e.g. running `membrane_timescaledb_reporter` for the first time) reload the page to get charts for all metrics.

## Copyright and License

Copyright 2020, [Software Mansion](https://swmansion.com/?utm_source=git&utm_medium=readme&utm_campaign=membrane_telemetry_grafana)

[![Software Mansion](https://logo.swmansion.com/logo?color=white&variant=desktop&width=200&tag=membrane-github)](https://swmansion.com/?utm_source=git&utm_medium=readme&utm_campaign=membrane_telemetry_grafana)

Licensed under the [Apache License, Version 2.0](LICENSE)






