# Proxy types

## Forward proxy

Forward proxies are typically used internally by large organizations, such as universities and enterprises, to:
1. Block employees to visit certain website
1. Monitoring employees online activity
1. Block malicious traffic from reaching an origin server
1. Improve the user experience by caching external site content

![reverse-proxy-02-1.jpg](./assets/reverse-proxy-02-1.jpg)

## Reverse proxy
1. Receiving a user connection request
1. Completing a TCP thee-way handshake, terminating the initial connection
1. Connecting with the origin server and forwarding the original request

![reverse-proxy-01-1.jpg](./assets/reverse-proxy-01-1.jpg)

5:55



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

