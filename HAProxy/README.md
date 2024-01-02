# Proxy types

## Forward proxy

Forward proxies are typically used internally by large organizations, such as universities and enterprises, to:
1. Block employees to visit certain website
1. Monitoring employees online activity
1. Block malicious traffic from reaching an origin server
1. Improve the user experience by caching external site content

![reverse-proxy-02-1.jpg](assests/reverse-proxy-01-1.jpg)

## Reverse proxy
1. Receiving a user connection request
1. Completing a TCP thee-way handshake, terminating the initial connection
1. Connecting with the origin server and forwarding the original request

![reverse-proxy-02-1.jpg](assests/reverse-proxy-02-1.jpg)

## Receiving proxy vs Loadbalancer

###  Reverse proxy:

A reverse proxy accepts a request from a client, forwards it to a server
that can fulfill it, and returns the server's response to the client.

* Increase security
* Increase scalability and flexibility
* Compression
* SSL termination
* Caching

### Loadbalancer

A load balancer distributes incoming client requests amount a group of
servers, in each case returning the response from the selected server to
appropriate client.

* Are most commonly deployed when a site needs multiple servers because
  the volume of requests is too much for a single server to handle
  efficiently 


## Layer 4 Loadbalancer

Forwards packets based on basic rules, if only knows `IP` and `Port`
and perhaps latency of the target service. That is what is available at
Layer 3/4. This load balancer doesn't look at the content so it doesn't
know the protocol whether its HTTP or not, it doesn't know the URL or
the path or the resource you are consuming or whether you are using
`GET` or `POST`

> A Layer 4 load balancer operates at the transport layer of the OSI
> model, making routing decisions based on factors like
> source/destination IP addresses and port numbers. It distributes
> incoming traffic across multiple servers, focusing on network and
> transport layer information (e.g., TCP or UDP). Unlike Layer 7 load
> balancers, Layer 4 balancers don't inspect application layer data,
> making them efficient for simple connection-based load balancing.

| **Pros of Layer 4 Load Balancers**      | **Cons of Layer 4 Load Balancers**          |
|---------------------------------------|--------------------------------------------|
| Efficient for basic load balancing.    | Limited application awareness.             |
| Low latency, suitable for performance-sensitive applications. | Inability to differentiate content types. |
| Versatile, can handle various types of traffic (TCP, UDP). | Less granular control over traffic routing. |
| Simple configuration and management.   | No SSL termination; backend servers handle encryption. |
| Effective for network layer decisions (IP addresses, port numbers). | Challenges in handling heterogeneous applications. |

## Layer 7 Loadbalancer

The load balancing operates at the high-level application layer, which
is responsible for the actual contents of the message. Layer 7 load
balancers route network traffic in a more complex manner, usually
applicable to TCP-Based traffic like HTTP. Unlike Layer 4, a Layer 7
load balancer terminates the network traffic and reads the message
within. It makes a decision based on the content of the message.
Afterwhich, it makes a new TCP connection to the selected upstream
server and writes the request to the server. It can also cache, Layer 4
isn't capable of doing so as it has no clue of what's in the packets.

| **Pros of Layer 7 Load Balancers** | **Cons of Layer 7 Load Balancers** |
|-------------------------------------|-------------------------------------|
| Application Awareness               | Higher processing overhead         |
| Content-Based Routing               | Potential for increased latency     |
| SSL Termination                     | Complex configuration              |
| Adaptability                        | Resource intensiveness              |
| Advanced Load Balancing Algorithms  | May not handle high connection counts as efficiently |

![assests/lbv4v7](assests/lbv4v7.png)

session 3

## Log formatting

```plaintext
# Define a custom log format named "custom_log"
log-format custom_log %[date] %[frontend_name] %[backend_name] %[server_name] \
    %[src] %[dst] %{+Q}r %ST %B %Ts %Tr %Tt %Hs %Hr %Ht %Tq %Tw %Tc %Tr %Tt %Tt

# Use the defined custom log format in a frontend or backend section
frontend example_frontend
    bind *:80
    default_backend example_backend
    log global
    log-format custom_log

backend example_backend
    server server1 192.168.1.1:8080
    server server2 192.168.1.2:8080
    log global
    log-format custom_log
```

Explanation of the log format elements:

- `%[date]`: Current date in local time.
- `%[frontend_name]`: Name of the frontend.
- `%[backend_name]`: Name of the backend.
- `%[server_name]`: Name of the server.
- `%[src]`: Source IP address.
- `%[dst]`: Destination IP address.
- `%{+Q}r`: Request line (URL) in double-quotes.
- `%ST`: HTTP status code.
- `%B`: Bytes read from the client.
- `%Ts`: Start time of the session.
- `%Tr`: Response time.
- `%Tt`: Total time.
- `%Hs`: Number of requests in session.
- `%Hr`: Number of responses in session.
- `%Ht`: Total number of requests and responses in session.
- `%Tq`: Total time waiting for the client request.
- `%Tw`: Total time in queues.
- `%Tc`: Total time waiting for the connection to establish to the final server.
- `%Tr`: Total time to receive the response from the final server.
- `%Tt`: Total time of the session.

