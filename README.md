
# News Stream Infrastructure

## Stack version
- Python 3
- Zookeeper version: 3.4.9
- Kafka version: 2.1.0 (Confluent 5.1.0)
- Kafka Schema Registry: Confluent 5.1.0
- [Kafka Schema Registry UI: 0.9.4](https://github.com/Landoop/schema-registry-ui)
- Kafka Rest Proxy: Confluent 5.1.0
- [Kafka Topics UI: 0.9.4](https://github.com/Landoop/kafka-topics-ui)
- Kafka Connect: Confluent 5.1.0
- [Kafka Connect UI: 0.9.4](https://github.com/Landoop/kafka-connect-ui)
- Zoonavigator: 0.5.1
- Mysql: 5.7

## News Stream Workflow

The following figure show the workflow of a News Stream Analyzer.

![](https://github.com/fabiana001/wallaroo_kafka_example/blob/master/imgs/workflow.png)

Data is initially stored in a mysql database and daily updated. Each time new data is inserted or updated in the mysql database:
1.  the Kafka Connect component polls it on a Kafka queue (on topic *quickstart-jdbc-PRO_clip_repository*). We set as polling strategy *timestamp* on the dataset attribute *insertdate* (for more information see [here](https://docs.confluent.io/current/connect/kafka-connect-jdbc/source-connector/source_config_options.html)).
2.  The News Analyzer component enriches the input message with other information
3.  The enriched message is sent to a kafka (on topic *news_genero*);
4. Finally the Kafka Connect component sends data having topic *news_genero* in the Elastic database.

## Run Enviroment with Docker

The following commands run dockers with a mysql server with 200 data record, an ElasticSearch server, Kafka and Kafka connector
```bash
> cd ./kafka_infrastructure_poc/docker
> chmod +x launch_demo.sh
> ./launch_demo.sh
```
For running the news analyzer component:
```bash
> cd ./kafka_infrastructure_poc/src
> python -m ./consumers/news_analyzer
```

## Web UIs

### Kafka Topics UI
It allows user to :
	- browse Kafka topics and understand what's happening on the cluster;
	- find topics and their metadata;
	- browse kafka messages and download them.

Web UI will be available at [localhost:8001](http://localhost:8001)
![](https://github.com/fabiana001/kafka_infrastructure_poc/blob/master/imgs/topic_ui.png)


### Kafka Schema Registry UI
It allow user to create, view, search and update  **Avro**  schemas of the Kafka cluster.

Web UI will be available at [localhost:8000](http://localhost:8000)

![](https://github.com/fabiana001/kafka_infrastructure_poc/blob/master/imgs/schema_registry_ui.png)


### Kafka Connect UI

This is a web tool for Kafka Connect for setting up and managing connectors for multiple connect clusters.

Web UI will be available at [localhost:8002](http://localhost:8002)

![](https://github.com/fabiana001/kafka_infrastructure_poc/blob/master/imgs/connect_ui.png)
