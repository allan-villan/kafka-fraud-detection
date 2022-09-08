# kafka-fraud-detection
A Streaming Fraud Detection System that monitors user activity and flags the transaction as either legit or fradulant using Kafka and Python.

## Requirements:
- Python: 3.10.4

- Kafka: 1.3.5

- Docker: 20.10.17

### Considerations:

Because a fraud detection system relies on streamed data to produce alerts as soon as possible,
Apache Kafka is used to handle the streamed data due to its capabilities and the robust community
surrounding this wonderful framework. 

### Architecture diagram:
![Kafka diagram drawio](https://user-images.githubusercontent.com/84660320/189236982-0eeee525-d074-48b4-aff5-b24ae9193658.png)

Two clusters are connected through a network to better separate processes:
- Source Cluster handles fraud detection
- Destination Cluster communicates the information processed by Source Cluster

### Project Layout:
<img width="311" alt="project_layout" src="https://user-images.githubusercontent.com/84660320/189238313-8e01728f-a13e-461a-80c0-ba39840d0c2f.png">

### Running the program:
We want to start the Destination Cluster first since it acts as the sink in this pipeline.

To spin up the Destination Cluster, run:
> docker-compose -f docker-compose.kafka.yml build && docker-compose -f docker-compose.kafka.yml up
<img width="757" alt="kafka_container_output" src="https://user-images.githubusercontent.com/84660320/189237794-fe89842e-1cb4-498b-928e-3b93d7a48e6e.png">

To run the Source Cluster that generates data to our Destination Cluster, run:
> docker-compose build && docker-compose up

This will cause the Generator to start producing random data that the Detector will process:
<img width="756" alt="generator_output" src="https://user-images.githubusercontent.com/84660320/189238459-cbe5c632-5ede-4e8c-8972-98579c05280d.png">

The Detector will print out a randomly generated source ID, target ID, and random amount "spent"
<img width="659" alt="detector_output" src="https://user-images.githubusercontent.com/84660320/189238925-5fdaee5d-0da3-4e76-a529-d5b8f9a671d9.png">

which is filtered for suspicious transactions. The image above only displays values under $900 because anything above it is being flagged as fraudulant. 
More logic can be applied to create a better fraud detection system, but that isn't the main focus for this project. Who knows, maybe I'll come back and 
implement a more advanced system :)

### Reflection:
Looking back, this was a great opportunity to learn about Kafka that utilized it in a reasonable example. I'm excited to apply this framework to 
problems that call for streamed data because to me, this is as live as data gets!
