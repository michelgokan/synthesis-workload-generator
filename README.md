# Introduction to Synthetic Service Mesh
Synthetic Service Mesh is a customizable, multi-threaded, and Docker/Kubernetes based synthetic workload that is used to generate different types of service meshes with various types of workloads. It can be used to emulate any type of service chains in a cluster and can be easily deployed in Kubernetes or Docker Swarm environments. For example, you can generate the service chain below:    

![Image of a service chain](./images/service_chain.png)

# Endpoints:
* /workload/cpu/:workloadSize?/:threadsCount?/:sendToNext? for CPU intensive workloads
* /workload/mem/:dataSize?/:threadsCount?/:sendToNext? for memory intensive workloads
* /workload/blkio/:fileSize?/:threadsCount?/:sendToNext? for disk intensive workloads
* /workload/net/:payloadSize? for network intensive workloads
* /workload/promisedNet/:payloadSize? for network intensive workloads
* /x for combined workloads


## CPU Intensive Workload: 
Generates <workloadSize> number of Diffie-Hellman keys and calculates their MD5 checksums! It then calls the NetworkIntensiveWorload with zero payload if sendToNext parameter is true.
## Memory Intensive Workload: 
Stores and release <dataSize>MB of data in the memory as a variable! It then calls the NetworkIntensiveWorload with zero payload if sendToNext parameter is true.
## Disk Intensive Workload: 
Writes <fileSize>MB of data in the disk and then deletes it! It then calls the NetworkIntensiveWorload with zero payload if sendToNext parameter is true.
## Network Intensive Workload:
Send <payloadSize>MB of data over network (POST HTTP) to N number of destinations specified in a comma separated environment variable NEXT_SERVICES_ADDRESSES!
It comes in 2 types:
 
### Normal:
Sends and exit!
### Promised:
Send, wait for response and then exit!

## Combined Workload: 
Run all workloads mentioned above once (with normal net)!

# Installation on Kubernetes
Simply use `kubectl apply -f synthetic-workload-generator.yaml` and access via `http://<your_nginx_address>/workload/`. Make sure to set NEXT_SERVICES_ADDRESSES environment variable in the yaml file for the network intensive workload and/or for properly forming your service mesh graph.
