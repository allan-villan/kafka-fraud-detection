# kafka-fraud-detection
A Streaming Fraud Detection System that monitors user activity and flags the transaction as either legit or fraudulent using Kafka and Python.

## Requirements:
- Python: 3.10.4

- Kafka: 1.3.5

- Docker: 20.10.17

### Considerations:

Because a fraud detection system relies on streamed data to produce alerts as soon as possible,
Apache Kafka is used to handle the incoming data due to its streaming capabilities and the robust community
surrounding this wonderful framework. 

### Architecture diagram:
![Kafka diagram drawio](https://user-images.githubusercontent.com/84660320/189236982-0eeee525-d074-48b4-aff5-b24ae9193658.png)

Two clusters are connected through a custom network labeled: ***kafka-network***
- The **Source Cluster** handles fraud detection
- The **Destination Cluster** communicates the information processed by the Source Cluster and regulates processes through **ZooKeeper**

### Project Layout:
To satisfy the architectural design, two Docker containers are used.

<img width="311" alt="project_layout" src="https://user-images.githubusercontent.com/84660320/189238313-8e01728f-a13e-461a-80c0-ba39840d0c2f.png">

### Running the program:
First of all, we want to start the **Destination Cluster** first since it acts as the sink in this pipeline.

> To spin up the Destination Cluster, run:

`docker-compose -f docker-compose.kafka.yml build && docker-compose -f docker-compose.kafka.yml up`

*This is what be what the terminal should look like...*
<img width="757" alt="kafka_container_output" src="https://user-images.githubusercontent.com/84660320/189237794-fe89842e-1cb4-498b-928e-3b93d7a48e6e.png">

Now, we need to spin up the **Source Cluster** to start feeding data to be processed.

> Use this command to run the **Source Cluster**:

`docker-compose build && docker-compose up`

The **Generator** will then begin generating random *source IDs*, *target IDs*, and random *amounts* "spent".

<img width="756" alt="generator_output" src="https://user-images.githubusercontent.com/84660320/189238459-cbe5c632-5ede-4e8c-8972-98579c05280d.png">

All transactions over $900 were marked as fraudulent by the **Topic** in the **Source Cluster**, so nothing over that amount should apear in the terminal. The data displayed below passed the fraudulent check and is being received by the **Destination Cluster**.

<img width="659" alt="detector_output" src="https://user-images.githubusercontent.com/84660320/189238925-5fdaee5d-0da3-4e76-a529-d5b8f9a671d9.png">

That concludes the project! 

### Reflection:
Looking back, this was a great opportunity to learn about Kafka that utilized it in a reasonable example. I'm excited to apply this framework to 
problems that call for streamed data because to me, this is as live as data gets!


More logic can be applied to create a better fraud detection system, but that isn't the main focus for this project. Who knows, maybe I'll come back and implement a more advanced system :)
