# Testing localy 

## Preprequisites
- kind  
- devspace 

## Local kubernetes cluster

Use `kind` to deploy local kubernetes cluster on your pc 

```
kind create cluster --name dev-magicflow
```

connect to it via context

```
kubectl cluster-info --context kind-dev-magicflow
```

<details>
  <summary>Spin up local k8s cluster</summary>

  ### Connect to it
```
 $ kind create cluster --name dev-magicflow
Creating cluster "dev-magicflow" ...
 ✓ Ensuring node image (kindest/node:v1.33.1) 🖼
 ✓ Preparing nodes 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
Set kubectl context to "kind-dev-magicflow"
You can now use your cluster with:

kubectl cluster-info --context kind-dev-magicflow

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
~/CTO2B/github/magicflow [feat/devspace] $ kubectl cluster-info --context kind-dev-magicflow
Kubernetes control plane is running at https://127.0.0.1:50072
CoreDNS is running at https://127.0.0.1:50072/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

```
</details>

# Deploy test env

- make sure you have Preprequisites and you are in magicflow dir `~/CTO2B/magicflow[main] $`
- run devspace

```
devspace dev -n magicflow
```
> **_NOTE:_**  above comand may fail by run it 1st time due to CRD's deploy take some time

<details>
  <summary>Check Deploy Output</summary>

  ### Start Dev Env
 ```
~/CTO2B/magicflow [main]$ devspace dev -n magicflow
info Using namespace 'magicflow'
info Using kube context 'kind-dev-magicflow'
info Created namespace: magicflow
pullsecret:gitlab-registry Ensuring image pull secret for registry: registry.gitlab.com...
pullsecret:gitlab-registry Created image pull secret magicflow/registry-credentials
deploy:strimzi Deploying chart oci://quay.io/strimzi-helm/strimzi-kafka-operator (strimzi) with helm...
deploy:kafka-debug Deploying chart  (kafka-debug) with helm...
deploy:kafka Deploying chart  (kafka) with helm...
deploy:crds Deploying chart  (crds) with helm...
deploy:magicflow Deploying chart  (magicflow) with helm...
deploy:kafka-debug Deployed helm chart (Release revision: 1)
deploy:kafka-debug Successfully deployed kafka-debug with helm
create_deployments: error deploying magicflow: unable to deploy helm chart: Release "magicflow" does not exist. Installing it now.
 error executing 'helm upgrade magicflow --values /var/folders/9f/k4x9t2xs321f39rzxnd985_h0000gn/T/2026060257 --install --namespace magicflow /Users/andrius/CTO2B/github/magicflow/k8s/magicflow --kube-context kind-dev-magicflow': Error: unable to build kubernetes objects from release manifest: resource mapping not found for name: "super-user" namespace: "magicflow" from "": no matches for kind "KafkaUser" in version "kafka.strimzi.io/v1beta2"
ensure CRDs are installed first

fatal exit status 1
~/CTO2B/magicflow [main]$ devspace dev -n magicflow
info Using namespace 'magicflow'
info Using kube context 'kind-dev-magicflow'
pullsecret:gitlab-registry Ensuring image pull secret for registry: registry.gitlab.com...
deploy:strimzi Deploying chart oci://quay.io/strimzi-helm/strimzi-kafka-operator (strimzi) with helm...
deploy:kafka Deploying chart  (kafka) with helm...
deploy:crds Deploying chart  (crds) with helm...
deploy:kafka-debug Deploying chart  (kafka-debug) with helm...
deploy:magicflow Deploying chart  (magicflow) with helm...
deploy:kafka Deployed helm chart (Release revision: 1)
deploy:kafka Successfully deployed kafka with helm
deploy:magicflow Deployed helm chart (Release revision: 1)
deploy:magicflow Successfully deployed magicflow with helm
deploy:kafka-debug Deployed helm chart (Release revision: 2)
deploy:kafka-debug Successfully deployed kafka-debug with helm
deploy:crds Deployed helm chart (Release revision: 2)
deploy:crds Successfully deployed crds with helm
deploy:strimzi Deployed helm chart (Release revision: 1)
deploy:strimzi Successfully deployed strimzi with helm
dev:magicflow Waiting for pod to become ready...
dev:magicflow Pod magicflow-devspace-99d478fcd-brlbx: MountVolume.SetUp failed for volume "ca" : secret "kafka-client-cluster-ca-cert" not found (FailedMount)
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: Init:0/1
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: Init:0/1
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: Init:0/1
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: Init:0/1
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: Init:0/1
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: Init:0/1
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: Init:0/1
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow DevSpace is waiting, because Pod magicflow-devspace-99d478fcd-brlbx has status: PodInitializing
dev:magicflow Selected pod magicflow-devspace-99d478fcd-brlbx
dev:magicflow sync  Sync started on: magicflow/ <-> /magicflow/
dev:magicflow sync  Waiting for initial sync to complete
dev:magicflow sync  Initial sync completed 
 ```

</details>

## deployment status
- entity operator for user management
- client-0 Kafka cluster one node
- kafka-debug pod for debuging kafka
- magicflow worker (job processor)
- strimzi kafka operator 


```
$ k get po -n magicflow
NAME                                          READY   STATUS    RESTARTS   AGE
kafka-client-entity-operator-cc6b69f6-2khft   1/1     Running   0          8m52s
kafka-client-kafka-client-0                   1/1     Running   0          9m45s
kafka-debug-54445c74c6-q5dk9                  1/1     Running   0          25s
magicflow-devspace-99d478fcd-brlbx            1/1     Running   0          10m
strimzi-cluster-operator-6f4fc4667c-4r4hj     1/1     Running   0          10m
```

# Testing 

> **_NOTE:_**  devspace will sync all your changes in code to container. If your code require new modules image rebuild required. Refer to devspace [build manual](https://www.devspace.sh/docs/configuration/images/build) 

<details>
  <summary>Get Report (Optional)</summary>

  ### Check topics and consumers 
```
k exec -it kafka-debug-54445c74c6-q5dk9 -n magicflow -- /bin/bash
Defaulted container "kafka-debug" out of: kafka-debug, kafka-truststore (init)
kafka-debug-54445c74c6-q5dk9:/apps/kafka_2.13-3.9.1/bin# ./kafka_debug.sh -a -g
Sasl: 1
Starting debug in kafka-client-kafka-bootstrap:9093 | Output file: /tmp/kafka_debug_log.txt
###################### Getting all Kafka topics...   ###################
cto2b.magicflow.workflows.worker.job cto2b.magicflow.workflows.worker.status
###################### Getting consumer groups...    ###################
cto2b.magicflow.group1
###################### Getting all group...          ###################

GROUP                  TOPIC                                PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                  HOST            CLIENT-ID
cto2b.magicflow.group1 cto2b.magicflow.workflows.worker.job 0          -               0               -               rdkafka-e4d54869-a367-43bd-a12e-62d6dc3f2c8a /10.244.0.7     rdkafka###################### Getting all groups members... ###################

GROUP                  CONSUMER-ID                                  HOST            CLIENT-ID       #PARTITIONS
cto2b.magicflow.group1 rdkafka-e4d54869-a367-43bd-a12e-62d6dc3f2c8a /10.244.0.7     rdkafka         1
###################### Getting topics description... ###################
Topic: __consumer_offsets	TopicId: Lrw71VxQQHmLdjzEMx2TKw	PartitionCount: 50	ReplicationFactor: 1	Configs: compression.type=producer,min.insync.replicas=1,cleanup.policy=compact,flush.ms=240000,segment.bytes=104857600,unclean.leader.election.enable=false
	Topic: __consumer_offsets	Partition: 0	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 1	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 2	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 3	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 4	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 5	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 6	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 7	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 8	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 9	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 10	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 11	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 12	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 13	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 14	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 15	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 16	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 17	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 18	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 19	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 20	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 21	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 22	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 23	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 24	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 25	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 26	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 27	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 28	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 29	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 30	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 31	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 32	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 33	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 34	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 35	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 36	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 37	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 38	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 39	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 40	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 41	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 42	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 43	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 44	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 45	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 46	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 47	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 48	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
	Topic: __consumer_offsets	Partition: 49	Leader: 0	Replicas: 0	Isr: 0	Elr: 	LastKnownElr:
Topic: cto2b.magicflow.workflows.worker.job	TopicId: 4L6Gc_BQT52_5yQpQVFGFQ	PartitionCount: 1	ReplicationFactor: 1	Configs: min.insync.replicas=1,flush.ms=240000,unclean.leader.election.enable=false
	Topic: cto2b.magicflow.workflows.worker.job	Partition: 0	Leader: 0	Replicas: 0	Isr: 0	Elr: LastKnownElr:
Topic: cto2b.magicflow.workflows.worker.status	TopicId: cu56fOokRrSUri4X-PNxxQ	PartitionCount: 1	ReplicationFactor: 1	Configs: min.insync.replicas=1,flush.ms=240000,unclean.leader.election.enable=false
	Topic: cto2b.magicflow.workflows.worker.status	Partition: 0	Leader: 0	Replicas: 0	Isr: 0	Elr: LastKnownElr:
###################### End of report...###################
```
</details>

<details>
  <summary>Publish Test job MSG</summary>

  ### Save Job For Publishing
```
kafka-debug-54445c74c6-q5dk9:/apps/kafka_2.13-3.9.1/bin# cat > job1
{"id":"b8e4e91f-3a71-46af-9f91-f01999c80734","created_at":"0001-01-01T00:00:00Z","updated_at":"0001-01-01T00:00:00Z","attributes":{"source":"dbot-controller","type":"job-input","date":"2025-05-15T09:55:28.81Z"},"message":"","data":{"created_at":"2025-05-15T09:55:28.81Z","input":{"from_jobs":[1],"mr_id":"1121","project_id":"25047365"},"job_id":"66288897-7c32-44ba-9d38-0f8aa1ba944a","name":"dummy_job","order":3,"output":{"error":{}},"status":"New","updated_at":"2025-05-15T09:55:28.81Z","workflow_id":"d2981e4e-b0fc-46be-90ac-4c5b17b1cb0b"}}
```
  ### Publish saved job 
```
kafka-debug-54445c74c6-q5dk9:/apps/kafka_2.13-3.9.1/bin# cat job1 | ./kafka-console-producer.sh   --topic cto2b.magicflow.workflows.worker.job   --bootstrap-server $KAFKA_BOOTSTRAP_SERVERS   --producer.config ../config/client_sasl.properties
```
> **_NOTE:_**  Json should be oneliner for publishing

  ### Json example 
```json
{
	"id": "b8e4e91f-3a71-46af-9f91-f01999c80734",
	"created_at": "0001-01-01T00:00:00Z",
	"updated_at": "0001-01-01T00:00:00Z",
	"attributes": {
		"source": "dbot-controller",
		"type": "job-input",
		"date": "2025-05-15T09:55:28.81Z"
	},
	"message": "",
	"data": {
		"created_at": "2025-05-15T09:55:28.81Z",
		"input": {
			"from_jobs": [
				1
			],
			"mr_id": "1121",
            "project_id": "25047365"
		},
		"job_id": "66288897-7c32-44ba-9d38-0f8aa1ba944a",
		"name": "dummy_job",
		"order": 3,
		"output": {
			"error": {}
		},
		"status": "New",
		"updated_at": "2025-05-15T09:55:28.81Z",
		"workflow_id": "d2981e4e-b0fc-46be-90ac-4c5b17b1cb0b"
	}
}
```  
</details>

<details>
  <summary>Job Process Output</summary>

  ### Log from magicflow
```
2025-05-20 18:49:18,713 - app.__name__ - INFO - KAFKA-SASL: Received message chunk: {"id":"b8e4e91f-3a71-46af-9f91-f01999c80734","created_at":"0001-01-01T00:00:00Z","updated_at":"0001-01-01T00:00:00Z","attributes":{"source":"dbot-controller","type":"job-input","date":"2025-05-15T09:55:28.81Z"},"message":"","data":{"created_at":"2025-05-15T09:55:28.81Z","input":{"from_jobs":[1],"mr_id":"1121","project_id":"25047365"},"job_id":"66288897-7c32-44ba-9d38-0f8aa1ba944a","name":"dummy_job","order":3,"output":{"error":{}},"status":"New","updated_at":"2025-05-15T09:55:28.81Z","workflow_id":"d2981e4e-b0fc-46be-90ac-4c5b17b1cb0b"}}

2025-05-20 18:49:18,724 - app.magicflow.jobs - DEBUG - Got command with id b8e4e91f-3a71-46af-9f91-f01999c80734

2025-05-20 18:49:18,725 - app.dummy - DEBUG - Doing dummy job msg = <magicflow.messaging.CommandMessage object at 0x7ffffe63fe80>

2025-05-20 18:49:18,744 - app.__name__ - INFO - Producing message to topic cto2b.magicflow.workflows.worker.status: {"id": "b8e4e91f-3a71-46af-9f91-f01999c80734", "created_at": "0001-01-01T00:00:00Z", "updated_at": "0001-01-01T00:00:00Z", "attributes": {"source": "dbot-controller", "type": "job-input", "date": "2025-05-15T09:55:28.81Z"}, "message": "", "data": {"created_at": "2025-05-15T09:55:28.81Z", "input": {"from_jobs": [1], "mr_id": "1121", "project_id": "25047365"}, "job_id": "66288897-7c32-44ba-9d38-0f8aa1ba944a", "name": "dummy_job", "order": 3, "output": {"error": {}, "mr_id:": "1121"}, "status": "Completed", "updated_at": "2025-05-15T09:55:28.81Z", "workflow_id": "d2981e4e-b0fc-46be-90ac-4c5b17b1cb0b"}}

2025-05-20 18:49:18,828 - app.__name__ - INFO - KAFKA-SASL: Message delivered to cto2b.magicflow.workflows.worker.status [0] at offset 0

2025-05-20 18:49:18,829 - app.__name__ - INFO - Message successfully produced to topic cto2b.magicflow.workflows.worker.status
```

we see status topic updated with `Completed` status. our job get input and pass input to output so we see output

`output": {"error": {}, "mr_id:": "1121"}`

</details>

## Cleanup 

```
$ kind delete cluster -n dev-magicflow
Deleting cluster "dev-magicflow" ...
Deleted nodes: ["dev-magicflow-control-plane"]
```