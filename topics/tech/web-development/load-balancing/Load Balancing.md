# Load Balancing

## What is load balancing?

Load balancing is a strategy to average the load across multiple servers to ensure that no single server is overwhelmed. It is a critical component of any high-traffic website or application, as modern applications need to deal with millions of requests simultaneously and send back the responses as texts, videos, images, etc., quickly and reliably. To deal with such high amount of traffic, many websites have a lot of resource servers with duplicated content. Load balancing is an infrastructure built between users and resource servers, acting as an orchestrator to evenly distribute the incoming requests to the servers.

## Benefits of load balancing

### Application availability

Load balancers automatically detect the server problems and redirect client requests to other servers. This increases fault tolerance of the system and decreases the application downtime.

Benefits:

* Run application server maintenance or upgrades without application downtime
* Provide automatic disaster recovery to backup sites
* Perform health checks and prevent issues that can cause downtime

### Application scalability

You can direct network traffic intelligently among multiple servers. Load balancing enables your application to handle thousands of client requests.

Benefits:

* Prevents traffic bottlenecks at any one server
* Predicts application traffic so that you can add or remove different servers, if needed
* Adds redundancy to your system so that you can scale it with confidence

### Application security

Load balancers provided by cloud providers have built-in security features that adds an extra layer of security to your application. They are a useful tool against distributed denial-of-service (DDoS) attacks. They also do the following:

* Monitor traffic and block malicious content
* Automatically redirect attack traffic to multiple backend servers to minimize impact
* Route traffic through a group of network firewalls for additional security

## Load balancing algorithms

### Static load balancing

| Algorithm | Description |
| --- | --- |
| Round-robin method | Doing load balancer on an authoritative name server, instead of on specialized hardware or software. The name server returns the IP addresses of different servers in the server farm turn by turn or in a round-robin fashion. |
| Weighted round-robin method | Assigns a weight to each server in the server pool. The weight indicates the processing capacity of the server. The server with a higher weight receives more requests. |
| IP hash method | Uses the client's IP address to determine which server to send the request to. The load balancer calculates the hash value of the client's IP address and then sends the request to the server with the corresponding hash value. |

### Dynamic load balancing

| Algorithm | Description |
| --- | --- |
| Least connections method | A connection is an open communication channel between a client and a server. When the client sends the first request to the server, they authenticate and establish an active connection between each other. In the least connection method, the load balancer checks which servers have the fewest active connections and sends traffic to those servers. This method assumes that all connections require equal processing power for all servers. |
| Weighted least connection method | Assumes that some servers can handle more connections than others. You can assign different weights or capacities to each server. The load balancers sends new requests to the server with least connections by capacity. |
| Least response time method | The response time is the time taken by the server to process the request and send the response back to the client. The least response time method combines the server response time active connections to determine the best server. The load balancer uses this algorithm to ensure fastest service to all users. |
| Resource-based time method | In the resource-based method, load balancers distribute traffic by analyzing the current server load. Specialized software called an agent runs on each server and calculates usage of server resources, such as its computing capacity and memory. Then, the load balancer checks the agent for sufficient free resources before distributing traffic to that server. |

## How load balancing works

Computers usually have their applications run on multiple servers, called a server farm. User requests first go to the route balancer. Then the load balancer routes the requests to the server most appropriate for handling the request.

![Load balancing AWS](../../../../images/load-balancing/load%20balancing%20AWS.png)

## Types of load balancing

## Types of load balancing technology

### Hardware load balancer

A hardware-based load balancer is a hardware appliance that can securely process and redirect gigabytes of traffic to hundreds of different servers. You can store it in your data centers and use virtualization to create multiple digital or virtual load balancers that you can centrally manage.

### Software load balancer

Software-based load balancers are applications that perform all load balancing functions. You can install them on any server or access them as a fully managed third-party service.

### Comparison between hardware load balancer and software load balancer

* **Hardware load balancer**
    * Needs initial investment, configuration, and ongoing maintenance
    * May be used only to handle peak-time traffic spikes, but not to full capacity
    * If traffic volume increases suddenly beyond current capacity, this will affect users until you purchase and set up another load balancer
* **Software load balancer**
    * More flexible and scalable
    * Scale up and down easily
    * More compatible with modern computing environments
    * Less expensive to set up, manage, and use over time

## References

* [Amazon AWS](https://aws.amazon.com/what-is/load-balancing/)