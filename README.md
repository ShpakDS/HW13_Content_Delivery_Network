# HW13_Content_Delivery_Network

## Setup
1. Run command `docker compose up`
2. Run command `curl -I localhost:8080/1.png` 2 times

## Results
```HW13_Content_Delivery_Network git:(main) ✗ curl -I localhost:8080/1.png                                          
HTTP/1.1 200 OK
Server: nginx/1.27.2
Date: Sun, 17 Nov 2024 13:40:51 GMT
Content-Type: image/png
Content-Length: 235916
Connection: keep-alive
Last-Modified: Sun, 17 Nov 2024 13:36:18 GMT
ETag: "3998c-6271be28eec07"
X-Cache-Status: MISS
Accept-Ranges: bytes

➜  HW13_Content_Delivery_Network git:(main) ✗ curl -I localhost:8080/1.png
HTTP/1.1 200 OK
Server: nginx/1.27.2
Date: Sun, 17 Nov 2024 13:40:52 GMT
Content-Type: image/png
Content-Length: 235916
Connection: keep-alive
Last-Modified: Sun, 17 Nov 2024 13:36:18 GMT
ETag: "3998c-6271be28eec07"
X-Cache-Status: HIT
Accept-Ranges: bytes
```

## NGINX Load Balancing Methods: Pros and Cons

| **Load Balancing Method** | **Advantages**                                                                                     | **Disadvantages**                                                                                  |
|---------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| **Round Robin**           | - Easy to set up and configure.                                                                  | - Does not take server load or health into consideration.                                          |
|                           | - Evenly distributes traffic across all servers.                                                 | - May direct requests to servers that are slow or overloaded.                                      |
| **Least Connections**     | - Optimizes load by sending traffic to servers with fewer active connections.                    | - Ignores server performance, potentially overloading less capable servers.                        |
|                           | - Improves traffic distribution dynamically.                                                     | - Requires a balanced environment to avoid inefficiencies.                                         |
| **IP Hash**               | - Maintains session persistence by routing the same client IP to the same server.                | - Uneven traffic distribution if client IPs are not evenly spread.                                 |
|                           | - Suitable for stateful applications that rely on session affinity.                              | - Not flexible for stateless applications or dynamic workloads.                                    |
| **Weighted Round Robin**  | - Allocates traffic based on server capacity, suitable for heterogeneous server environments.     | - Manual configuration of weights can become tedious and may require frequent updates.             |
|                           | - Improves efficiency over basic Round Robin for servers with different specifications.           | - Misconfigured weights can reduce load balancing effectiveness.                                   |
| **Consistent Hashing**    | - Ensures predictable routing of keys (e.g., session IDs) to the same server, ideal for caching.  | - Adding or removing servers may cause cache misses and uneven traffic.                            |
|                           | - Excellent for caching and distributed database systems.                                         | - Demands careful implementation and knowledge of the hashing algorithm.                           |
| **Least Time**            | - Prioritizes servers with the fastest response time and lowest active connections.               | - Only available with NGINX Plus or specific third-party modules.                                  |
|                           | - Enhances responsiveness and balances server speed and load.                                    | - Adds configuration complexity and potential overhead.                                            |
| **Failover (Backup)**     | - Ensures high availability by redirecting requests to backup servers during primary failures.    | - Backup servers remain idle, leading to inefficient resource usage when no failures occur.         |
|                           | - Critical for fault-tolerant systems in production environments.                                | - May increase response time during failover transitions.                                          |
| **Health Check**          | - Sends traffic only to healthy servers, increasing reliability and uptime.                      | - Often requires additional modules (e.g., `nginx_http_upstream_check_module`).                    |
|                           | - Vital for production setups with fluctuating server health.                                   | - Increases setup and monitoring complexity.                                                       |