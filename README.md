# Lo2FE Docker Connector

This docker image includes NodeRed connector allowing connection to LiveObject FIFO and everytime a message is published in Live Object FIFO, the message will be delivered and published to Flexible Engine DIS (Data Injection Service).

## 0. Docker Repository

 The Docker Image for Lo2FE Docker Connector is maintained in Docker Hub Public repository available at  [Lo2FE Docker Image at DockerHub Repository](https://hub.docker.com/r/enamhee/lo2fe)

## 1. Pre-Requisite

The following service need to be provisionned on Flexible Engine and Live Object 
  - Live Object Account 
  - Live Object FIFO
  - Live Object API Key (https://liveobjects.orange-business.com/doc/html/lo_manual_v2.html#REST_MQTT_APP)

### 1.1 Live Object FIFO and API 

The following information are required to connect and collect the Live Object FIFO messages:

| Information | Description | Value Exemple |
| --- | --- | --- |
| Live Object API Key | API Key with relevant right to read Live Object FIFO | d2bf5cf323894b4bae08beac9cc4276b |
| Live Object URI | URI to connect to Live Object | ssl://liveobjects.orange-business.com:8883 |
| Live Object FIFO | Live Object FIFO (First In First Out) Queue where the message are stored| fifo/Flexible_Engine_FIFO |


### 1.2 Flexible Engine 

The following information are required to connect to Flexible Engine Data Injection Services (DIS):

| Information | Description | Example Value |
| --- | --- | --- |
| DIS EndPoint | Endpoint to connect to Flexible Engine DIS Service. This is information available at [Flexible Engine EndPoint](https://docs.prod-cloud-ocb.orange-business.com/endpoint/index.html) | https://dis.eu-west-0.prod-cloud-ocb.orange-business.com" |
| DIS Project | Flexible Engine Project into which DIS Service has been created | eu-west-0 |
| DIS StreamName | Flexible Engine Project DIS StreamName. The StreamName is created later in the procedure | ie. dis-liveobject |
| Flexible Engine User Name | Flexible Engine UserName available from Flexible Engine Technical Console. Refer to [Flexible Engine My Credential](https://docs.prod-cloud-ocb.orange-business.com/usermanual/ac/en-us_topic_0046783936.html)| ie. jame.durand |
| Flexible Engine Password | Flexible Engine Password available from Flexible Engine Technical Console.Refer to [Flexible Engine My Credential](https://docs.prod-cloud-ocb.orange-business.com/usermanual/ac/en-us_topic_0046783936.html) | ie. password |

## 2. (Optional) Create Cloud Container Engine (CCE) and Elastic Load Balancer (ELB) on Flexible Engine

In order to run the docker image on Flexible Engine, Flexible Engine Cloud Container Engine (CCE) can be used to deploy the connector container images

### 2.1 Create the Elastic Load Balancer (ELB)

The detail description on how to create an Elastic Load Balancer with associated EIP in available here [(Create Enhanced Elastic Load Balancer)](https://docs.prod-cloud-ocb.orange-business.com/usermanual/elb/en-us_topic_0015479967.html)

### 2.1 Create the Kubernetes Cluster

The detail description on how to create a Kubernetes Cluster in available here [(Create Hybrid Cluster with CCE)](https://docs.prod-cloud-ocb.orange-business.com/usermanual2/cce/cce_01_0028.html)

### 2.2 Create the Secret to store sensitive data

For security reason, the sensitive Data such as API KEYs, Login, Password are kept inside Secret and the value are encoded using base64 
In order to encode the data, you can use online encoding tools (https://www.base64encode.org/)

| Information | Description | Example Value (ENCODED in base64) | Example Value (NON ENCODED base64) |
| --- | --- | --- | --- |
| LO_API_KEY | Life Object API Key | ZDJiZjVjZjMyMzg5NGI0YmFlMDhiZWFjOWNjNDI3NmU= | d2bf5cf323894b4bae08beac9cc4276e |
| DIS_TOKEN_USERNAME | Flexible Engine Credential correspond to User | ZXJuZXN0Lm5hbS5oZWUx= |  jame.durand |
| DIS_TOKEN_PASSWORD | Flexible Engine Credential correspond to Password  | NVR2bWRsbWRtczE1ZUAx= | password |


## 3. Deploy the Lo2FE_Connector on Flexible Engine CCE

The Lo2FE connector is available as docker image available on Docker Hub at [here](https://hub.docker.com/u/enamhee).

It is possible to deploy the Lo2FE connector using two methods:
  - Through Flexible Engine CCE User Interface
  - Through Kubectl Command Line


### 3.1 Deploying Through Flexible Engine CCE User Interface

You can use Flexible Engine CCE GUI to deploy the Lo2FE Connector Docker Image

Below is the Environment Parameter required by the images


| Environment Parameter | Description | Value |
| --- | --- | --- |
| LO_URI | URI to connect to Live Object | ssl://liveobjects.orange-business.com:8883 |
| LO_API_KEY | API Key with relevant right to read Live Object FIFO | Value is stored in Secret *lo2fe-secret* (LO_API_KEY)| 
| LO_FIFO | Live Object FIFO (First In First Out) Queue where the message are stored| ie. fifo/Flexible_Engine_FIFO |
| DIS_ENDPOINT | DIS End Point. Please refer to  [Flexible Engine EndPoint](https://docs.prod-cloud-ocb.orange-business.com/endpoint/index.html) | https://dis.eu-west-0.prod-cloud-ocb.orange-business.com |  
| DIS_PROJECT | Flexible Engine Project Id or Region where the DIS has been created | eu-west-0 |  
| DIS_STREAMNAME | Flexible Engine Project DIS StreamName | ie. dis-liveobject |  
| DIS_TOKEN_USERNAME | Flexible Engine UserName available from Flexible Engine Technical Console | Value is stored in Secret *lo2fe-secret* (DIS_TOKEN_USERNAME)|  
| DIS_TOKEN_PASSWORD | Flexible Engine Password available from Flexible Engine Technical Console | Value is stored in Secret *lo2fe-secret* (DIS_TOKEN_PASSWORD)|  

### 3.2 Deploying Through Kubectl commande line

You can deploy Lo2FE Connector Docker Image using Kubectl commande line

You will need 

## 4. Logging to Admin Console

Login : admin
Password : lo2fepassword

## 4. (Optional) Build the Lo2FE_Docker image from GIThub repository
