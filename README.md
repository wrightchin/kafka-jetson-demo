### NVIDIA Jetson Nnao with Red Hat OpenShift and AMQ Kafka Bridge
- Based on [Harr Cascade by OpenCV](https://docs.opencv.org/3.4/db/d28/tutorial_cascade_classifier.html)

### Requirments 
Red Hat Openshift Container Platform
- Red Hat Integration - AMQ Streams: 2.2.0-1 

Jetson Nano
- Ubuntu: 18.04
- Docker: 20.10.20
- Docker Compose: v2.12.0
- USB camera

### Usages
1. Create Kafka cluster in OpenShift.
2. Create Kafka Bridge in OpenShift.

After the Kafka Bridge createion, expose the Kafka Bridge.
```sh
$ oc expose svc <bridge_name>-bridge-service
route.route.openshift.io/my-bridge-bridge-service exposed
```

Obtaining the route of the Kafka Bridge.
```sh
$ oc get route my-bridge-bridge-service -o template --template '{{.spec.host}}'
```

Check that the Kafka Bridge is working.
```sh
$ curl -v http://<route>/healthy
```

3. Receiving Message from Jetson (Consumers).
```sh
$ oc -n <project_name> run kafka-consumer -ti \
--image=registry.redhat.io/amq7/amq-streams-kafka-31-rhel8:2.1.0 \
--rm=true \
--restart=Never \
-- bin/kafka-console-consumer.sh \
--bootstrap-server  <kafka_cluster>-bootstrap:9092 \
--topic my-topic \
--from-beginning
```

4. Start the Jetson Nano (Producer).
```sh
git clone this project and change the path of the post request,
$ docker-compsoe up
```
Done!