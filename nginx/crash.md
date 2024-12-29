# Introduction

## **1. What is NGINX?**
NGINX is an open-source, high-performance web server, reverse proxy, load
balancer, and HTTP cache. It’s designed to handle a large number of concurrent
connections efficiently, making it ideal for modern web applications. NGINX is
known for its:
- Speed and low resource usage
- Scalability for high-traffic websites
- Flexibility in handling static content, dynamic content, and proxying

---

## **2. What is NOT NGINX?**
- **Not a programming language:** NGINX is not used to write application logic (e.g., Python, JavaScript)
- **Not a database:** It doesn’t store or manage data like MySQL or MongoDB.
- **Not a full application server:** While it can proxy requests to application servers (e.g., Node.js, Django), it doesn’t execute application code itself.
- **Not a replacement for Apache in all cases:** While NGINX is often faster and more efficient, Apache has features (e.g., .htaccess support) that some users may prefer.

---

## **3. Popular Scenarios for NGINX**
- **Web Server:** Serving static content (HTML, CSS, JS, images).
- **Reverse Proxy:** Forwarding requests to backend servers (e.g., Node.js, Python, Java).
- **Load Balancer:** Distributing traffic across multiple servers for high availability.
- **SSL/TLS Termination:** Handling HTTPS encryption for secure communication.
- **Caching:** Accelerating content delivery by caching static or dynamic content.
- **API Gateway:** Managing and routing API requests.
- **Media Streaming:** Delivering video or audio content efficiently.

---

## **4. Is NGINX Plus Really Different from NGINX Free Edition?**
Yes, NGINX Plus is the commercial version of NGINX, offering additional features and support:
| **Feature**               | **NGINX (Free)**          | **NGINX Plus**               |
|---------------------------|--------------------------|------------------------------|
| **Cost**                  | Free                    | Paid (subscription-based)    |
| **Support**               | Community support       | Official support from NGINX  |
| **Advanced Load Balancing**| Basic                  | Advanced algorithms (e.g., least time) |
| **Monitoring**            | Limited                 | Real-time monitoring dashboard |
| **API Management**        | No                      | Yes                          |
| **Configuration Management**| Manual                | Dynamic reconfiguration via API |
| **High Availability**     | Manual setup            | Built-in active-active HA    |

---

## **5. Is OpenResty the Same Thing?**
No, OpenResty is not the same as NGINX. OpenResty is a **bundle** that includes
NGINX, LuaJIT (a Lua scripting engine), and additional modules. It’s designed
for building dynamic web applications and APIs using Lua scripting directly
within NGINX. OpenResty extends NGINX’s capabilities but is not a replacement
for it.

---

##**6. Comparison of NGINX, OpenResty, and HAProxy**

| **Feature**               | **NGINX**                | **OpenResty**               | **HAProxy**                 |
|---------------------------|--------------------------|-----------------------------|-----------------------------|
| **Primary Use Case**      | Web server, reverse proxy, load balancer | Dynamic web applications, APIs | Load balancer, TCP/HTTP proxy |
| **Scripting Support**     | Limited (via Lua module) | Full Lua scripting support  | No                          |
| **Performance**           | High                    | High (with Lua overhead)    | Very high                   |
| **Ease of Configuration** | Moderate                | Moderate (requires Lua knowledge) | Easy                        |
| **Load Balancing**        | Basic to advanced       | Basic to advanced           | Advanced (focus on LB)      |
| **Caching**               | Yes                     | Yes                         | No                          |
| **SSL/TLS Termination**   | Yes                     | Yes                         | Yes                         |
| **Community Support**     | Large                   | Smaller (focused on Lua)    | Large                       |
| **Commercial Support**    | NGINX Plus              | OpenResty Inc.              | HAProxy Enterprise          |
| **Best For**              | General-purpose web server, reverse proxy | Dynamic applications, APIs  | High-performance load balancing |

---

## **Key Takeaways**
- **NGINX** is a versatile, high-performance web server and reverse proxy.
- **NGINX Plus** adds enterprise features and support.
- **OpenResty** extends NGINX with Lua scripting for dynamic applications.
- **HAProxy** is focused on high-performance load balancing and proxying.

Each tool has its strengths, and the choice depends on your specific use case. For example:
- Use **NGINX** for serving static content or as a reverse proxy
- Use **OpenResty** if you need Lua scripting for dynamic behavior. 
- Use **HAProxy** if your primary focus is load balancing.


# NGINX Components

NGINX is composed of several key components that work together to deliver its
functionality as a web server, reverse proxy, load balancer, and more.
Understanding these components will help you configure and optimize NGINX
effectively. Here’s a breakdown of the main components:

---

## **1. NGINX Master Process**
- **Role:** The master process is the main process that controls the NGINX server.
- **Responsibilities:** 
  - Reads and validates the configuration file. 
  - Manages worker processes (starts, stops, or reloads them).
  - Does not handle client requests directly.
- **Example Command:**
  ```bash
  sudo nginx -s reload  # Reloads the configuration without downtime
  ```

---

## **2. NGINX Worker Processes**
- **Role:** Worker processes handle client requests (e.g., serving static files, proxying requests).
- **Responsibilities:** 
  - Handle multiple connections simultaneously using an event-driven, non-blocking architecture.
  - Execute the actual work of serving requests.
- **Key Features:** 
  - Each worker process is single-threaded but can handle thousands of connections. 
  - The number of worker processes is configurable (default is usually 1).
- **Configuration Example:**
  ```nginx
  worker_processes 4;  # Use 4 worker processes
  ```

---

## **3. Configuration File**
- **Role:** The configuration file defines how NGINX behaves.
- **Location:** Typically located at `/etc/nginx/nginx.conf`.
- **Structure:**
  - **Main Context:** Global settings (e.g., `worker_processes`, `error_log`).
  - **Events Context:** Configuration for connection handling (e.g., `worker_connections`). 
  - **HTTP Context:** Settings for HTTP and HTTPS traffic.
  - **Server Context:** Defines virtual servers (e.g., `server_name`, `listen`).
  - **Location Context:** Specifies how to handle specific URLs.
- **Example:**
  ```nginx
  worker_processes 2;
  events {
      worker_connections 1024;
  }
  http {
      server {
          listen 80;
          server_name example.com;
          location / {
              root /var/www/html;
          }
      }
  }
  ```

> ---
> 
> ### **Full NGINX Configuration File**
> 
> ```nginx
> # Main Context
> user www-data;
> worker_processes auto;
> error_log /var/log/nginx/error.log warn;
> pid /var/run/nginx.pid;
> 
> # Events Context
> events {
>     worker_connections 1024;
>     multi_accept on;
>     use epoll;
> }
> 
> # HTTP Context
> http {
>     # Basic Settings
>     include /etc/nginx/mime.types;
>     default_type application/octet-stream;
>     sendfile on;
>     keepalive_timeout 65;
>     client_max_body_size 10M;
> 
>     # Logging Format
>     log_format main '$remote_addr - $remote_user [$time_local] "$request" '
>                     '$status $body_bytes_sent "$http_referer" '
>                     '"$http_user_agent" "$http_x_forwarded_for"';
>     access_log /var/log/nginx/access.log main;
> 
>     # Gzip Compression
>     gzip on;
>     gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
> 
>     # SSL Settings
>     ssl_protocols TLSv1.2 TLSv1.3;
>     ssl_ciphers HIGH:!aNULL:!MD5;
>     ssl_prefer_server_ciphers on;
> 
>     # Cache Settings
>     proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m max_size=1g inactive=60m use_temp_path=off;
> 
>     # Upstream for Load Balancing
>     upstream backend {
>         least_conn;
>         server 192.168.1.101:8080;
>         server 192.168.1.102:8080;
>         keepalive 32;
>     }
> 
>     # Default Server Block (HTTP)
>     server {
>         listen 80;
>         server_name example.com www.example.com;
> 
>         # Redirect HTTP to HTTPS
>         return 301 https://$host$request_uri;
>     }
> 
>     # HTTPS Server Block
>     server {
>         listen 443 ssl;
>         server_name example.com www.example.com;
> 
>         # SSL Certificates
>         ssl_certificate /etc/nginx/ssl/cert.pem;
>         ssl_certificate_key /etc/nginx/ssl/key.pem;
> 
>         # Root Directory for Static Files
>         root /var/www/html;
>         index index.html index.htm;
> 
>         # Serve Static Files
>         location / {
>             try_files $uri $uri/ =404;
>         }
> 
>         # Reverse Proxy to Backend
>         location /api {
>             proxy_pass http://backend;
>             proxy_set_header Host $host;
>             proxy_set_header X-Real-IP $remote_addr;
>             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>             proxy_set_header X-Forwarded-Proto $scheme;
> 
>             # Enable Caching
>             proxy_cache my_cache;
>             proxy_cache_valid 200 302 10m;
>             proxy_cache_valid 404 1m;
>         }
> 
>         # Serve Static Files from a Different Directory
>         location /static {
>             alias /var/www/static;
>             expires 30d;
>             access_log off;
>         }
> 
>         # Deny Access to Hidden Files
>         location ~ /\.ht {
>             deny all;
>         }
> 
>         # Custom Error Pages
>         error_page 404 /404.html;
>         location = /404.html {
>             internal;
>         }
> 
>         error_page 500 502 503 504 /50x.html;
>         location = /50x.html {
>             internal;
>         }
>     }
> 
>     # Additional Server Block for Another Domain
>     server {
>         listen 80;
>         server_name another.com;
> 
>         location / {
>             proxy_pass http://backend;
>             proxy_set_header Host $host;
>             proxy_set_header X-Real-IP $remote_addr;
>             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>         }
>     }
> }
> ```
> 
> ---
> 
> ### **Explanation of the Configuration**
> 
> 1. **Main Context:**
>    - `user`: Runs worker processes as the `www-data` user.
>    - `worker_processes`: Automatically sets the number of worker processes based on CPU cores.
>    - `error_log`: Logs errors to `/var/log/nginx/error.log`.
> 
> 2. **Events Context:**
>    - `worker_connections`: Allows each worker process to handle up to 1024 connections.
>    - `multi_accept`: Enables accepting multiple connections at once.
>    - `use epoll`: Uses the `epoll` event model for efficient connection handling.
> 
> 3. **HTTP Context:**
>    - `include mime.types`: Includes MIME types for file extensions.
>    - `default_type`: Sets the default MIME type for responses.
>    - `sendfile`: Enables efficient file serving.
>    - `keepalive_timeout`: Sets the timeout for keep-alive connections.
>    - `client_max_body_size`: Limits the size of client request bodies to 10MB.
>    - `log_format`: Defines a custom log format.
>    - `gzip`: Enables gzip compression for specified file types.
> 
> 4. **SSL/TLS Settings:**
>    - `ssl_protocols`: Enables TLS 1.2 and 1.3.
>    - `ssl_ciphers`: Specifies secure cipher suites.
>    - `ssl_prefer_server_ciphers`: Ensures the server’s cipher preferences are used.
> 
> 5. **Cache Settings:**
>    - `proxy_cache_path`: Configures a cache zone named `my_cache` with a maximum size of 1GB.
> 
> 6. **Upstream Block:**
>    - `upstream backend`: Defines a group of backend servers for load balancing using the `least_conn` algorithm.
> 
> 7. **Server Blocks:**
>    - **HTTP Server:** Redirects all HTTP traffic to HTTPS.
>    - **HTTPS Server:** Serves static files, reverse proxies to a backend, and caches responses.
>    - **Additional Server:** Proxies requests for another domain.
> 
> 8. **Location Blocks:**
>    - `/`: Serves static files from `/var/www/html`.
>    - `/api`: Reverse proxies requests to the backend and caches responses.
>    - `/static`: Serves static files from `/var/www/static` with caching headers.
>    - `~ /\.ht`: Denies access to hidden files (e.g., `.htaccess`).
> 
> 9. **Error Pages:**
>    - Custom error pages for `404` and `50x` errors.
> 
> ---
> 
> ### **How to Use This Configuration**
> 1. Save the file as `/etc/nginx/nginx.conf`.
> 2. Test the configuration for syntax errors:
>    ```bash
>    sudo nginx -t
>    ```
> 3. Reload NGINX to apply the changes:
>    ```bash
>    sudo systemctl reload nginx
>    ```
> 
> ---
> 
> This configuration is a robust starting point for most use cases. Let me know if you need help customizing it further! 🚀

---

## **4. Modules**
- **Role:** Modules extend NGINX’s functionality.
- **Types:**
  - **Core Modules:** Built-in modules (e.g., `http`, `stream`).
  - **Third-Party Modules:** Add additional features (e.g., Lua scripting with the `ngx_http_lua_module`).
- **Examples:** 
  - `ngx_http_proxy_module`: For reverse proxying.
  - `ngx_http_ssl_module`: For SSL/TLS termination. 
  - `ngx_http_rewrite_module`: For URL rewriting.

---

## **5. Upstream Blocks**
- **Role:** Defines groups of backend servers for load balancing or proxying.
- **Usage:**
  - Used in conjunction with the `proxy_pass` directive.
  - Supports load balancing algorithms (e.g., round-robin, least connections).
- **Example:**
  ```nginx
  upstream backend {
      server 192.168.1.101;
      server 192.168.1.102;
  }
  server {
      location / {
          proxy_pass http://backend;
      }
  }
  ```

---

## **6. Logs**
- **Role:** Logs provide insights into NGINX’s operation and help with debugging.
- **Types:**
  - **Access Logs:** Record all incoming requests (e.g., `/var/log/nginx/access.log`).
  - **Error Logs:** Record errors and warnings (e.g., `/var/log/nginx/error.log`).
- **Configuration Example:**
  ```nginx
  http {
      access_log /var/log/nginx/access.log;
      error_log /var/log/nginx/error.log;
  }
  ```

---

## **7. Cache**
- **Role:** NGINX can cache content to improve performance. 
- **Usage:**
  - Caching static files or proxied responses.
  - Configurable cache size, location, and expiration.
- **Example:**
  ```nginx
  http {
      proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m max_size=1g;
      server {
          location / {
              proxy_cache my_cache;
              proxy_pass http://backend;
          }
      }
  }
  ```

---

## **8. SSL/TLS**
- **Role:** NGINX can handle SSL/TLS termination for secure communication.
- **Usage:**
  - Encrypts traffic between clients and the server.
  - Supports modern protocols like TLS 1.3. 
- **Example:**
  ```nginx
  server {
      listen 443 ssl;
      server_name example.com;
      ssl_certificate /etc/nginx/ssl/cert.pem;
      ssl_certificate_key /etc/nginx/ssl/key.pem;
  }
  ```

---

## **9. Variables**
- **Role:** Variables allow dynamic behavior in NGINX configurations.
- **Examples:** 
  - `$host`: The requested hostname.
  - `$remote_addr`: The client’s IP address.
  - `$uri`: The requested URI.
- **Usage Example:**
  ```nginx
  location / {
      return 200 "Hello, $host!";
  }
  ```

---

## **10. Directives**
- **Role:** Directives are configuration commands that define NGINX’s behavior. 
- **Examples:** 
  - `listen`: Specifies the port and IP address to listen on.
  - `server_name`: Defines the server’s hostname.
  - `location`: Configures how specific URLs are handled.
  - `proxy_pass`: Forwards requests to a backend server.

> ## Directives cheatsheet
>  
>  ---
>  
>  ### **Main Context Directives**
>  | **Directive**            | **Description**                                                                 |
>  |--------------------------|---------------------------------------------------------------------------------|
>  | `user`                   | Defines the user and group under which worker processes run.                    |
>  | `worker_processes`       | Sets the number of worker processes (default: 1).                               |
>  | `error_log`              | Specifies the file and log level for error logs.                                |
>  | `pid`                    | Defines the file where the process ID (PID) of the master process is stored.    |
>  | `include`                | Includes other configuration files.                                             |
>  | `load_module`            | Loads a dynamic module.                                                         |
>  
>  ---
>  
>  ### **Events Context Directives**
>  | **Directive**            | **Description**                                                                 |
>  |--------------------------|---------------------------------------------------------------------------------|
>  | `worker_connections`     | Sets the maximum number of simultaneous connections per worker process.         |
>  | `use`                    | Specifies the connection processing method (e.g., `epoll`, `kqueue`).           |
>  | `multi_accept`           | Allows a worker to accept all new connections at once (default: off).           |
>  
>  ---
>  
>  ### **HTTP Context Directives**
>  | **Directive**            | **Description**                                                                 |
>  |--------------------------|---------------------------------------------------------------------------------|
>  | `server`                 | Defines a virtual server.                                                      |
>  | `listen`                 | Specifies the IP address and port to listen on.                                |
>  | `server_name`            | Defines the server’s hostname(s).                                              |
>  | `root`                   | Sets the root directory for requests.                                          |
>  | `index`                  | Defines the default file to serve for directory requests.                      |
>  | `location`               | Configures how specific URLs are handled.                                      |
>  | `proxy_pass`             | Forwards requests to a backend server.                                         |
>  | `proxy_set_header`       | Modifies or adds headers when proxying requests.                               |
>  | `upstream`               | Defines a group of backend servers for load balancing.                         |
>  | `access_log`             | Specifies the file and format for access logs.                                 |
>  | `error_log`              | Specifies the file and log level for error logs.                               |
>  | `gzip`                   | Enables or disables gzip compression.                                          |
>  | `ssl_certificate`        | Specifies the SSL certificate file.                                            |
>  | `ssl_certificate_key`    | Specifies the SSL private key file.                                            |
>  | `ssl_protocols`          | Defines the supported SSL/TLS protocols.                                       |
>  | `ssl_ciphers`            | Specifies the SSL/TLS cipher suite.                                            |
>  | `keepalive_timeout`      | Sets the timeout for keep-alive connections.                                   |
>  | `client_max_body_size`   | Limits the maximum size of the client request body.                            |
>  | `rewrite`                | Rewrites URLs using regular expressions.                                       |
>  | `try_files`              | Checks for the existence of files in a specified order.                        |
>  | `add_header`             | Adds custom headers to responses.                                              |
>  | `expires`                | Sets caching headers for static content.                                       |
>  
>  ---
>  
>  ### **Location Context Directives**
>  | **Directive**            | **Description**                                                                 |
>  |--------------------------|---------------------------------------------------------------------------------|
>  | `alias`                  | Maps a URL path to a file system path.                                         |
>  | `deny`                   | Denies access to a specific location.                                          |
>  | `allow`                  | Allows access to a specific location.                                          |
>  | `proxy_cache`            | Enables caching for proxied responses.                                         |
>  | `proxy_cache_valid`      | Sets caching duration for specific response codes.                             |
>  | `proxy_cache_bypass`     | Defines conditions to bypass the cache.                                        |
>  | `proxy_no_cache`         | Defines conditions to prevent caching.                                         |
>  | `auth_basic`             | Enables HTTP Basic Authentication.                                             |
>  | `auth_basic_user_file`   | Specifies the file containing usernames and passwords for Basic Authentication.|
>  
>  ---
>  
>  ### **Upstream Context Directives**
>  | **Directive**            | **Description**                                                                 |
>  |--------------------------|---------------------------------------------------------------------------------|
>  | `server`                 | Defines a backend server in an upstream block.                                 |
>  | `least_conn`             | Uses the least connections load balancing algorithm.                           |
>  | `ip_hash`                | Uses IP-based load balancing for session persistence.                          |
>  | `zone`                   | Defines a shared memory zone for upstream configurations.                      |
>  | `keepalive`              | Sets the number of idle keepalive connections to upstream servers.             |
>  
>  ---
>  
>  ### **Stream Context Directives**
>  | **Directive**            | **Description**                                                                 |
>  |--------------------------|---------------------------------------------------------------------------------|
>  | `listen`                 | Specifies the IP address and port to listen on for TCP/UDP traffic.            |
>  | `proxy_pass`             | Forwards TCP/UDP traffic to a backend server.                                  |
>  | `proxy_timeout`          | Sets the timeout for proxied connections.                                      |
>  | `proxy_connect_timeout`  | Sets the timeout for establishing a connection to the backend server.          |
>  
>  ---
>  
>  ### **Mail Context Directives**
>  | **Directive**            | **Description**                                                                 |
>  |--------------------------|---------------------------------------------------------------------------------|
>  | `listen`                 | Specifies the IP address and port to listen on for mail traffic.               |
>  | `protocol`               | Defines the mail protocol (e.g., `imap`, `pop3`, `smtp`).                      |
>  | `proxy_pass`             | Forwards mail traffic to a backend server.                                     |
>  | `auth_http`              | Specifies an HTTP authentication server for mail.                              |
>  
>  ---
>  
>  ### **Miscellaneous Directives**
>  | **Directive**            | **Description**                                                                 |
>  |--------------------------|---------------------------------------------------------------------------------|
>  | `map`                    | Creates variables whose values depend on other variables.                      |
>  | `geo`                    | Creates variables based on the client’s IP address.                            |
>  | `limit_req`              | Limits the request processing rate.                                            |
>  | `limit_conn`             | Limits the number of connections per client.                                   |
>  | `return`                 | Stops processing and returns a specified status code or URL.                   |
>  | `rewrite`                | Rewrites URLs using regular expressions.                                       |
>  | `if`                     | Provides conditional logic (use with caution).                                 |
>  
>  ---
>  
>  ### **Example Configuration with Directives**
>  ```nginx
>  worker_processes 2;
>  events {
>      worker_connections 1024;
>  }
>  http {
>      server {
>          listen 80;
>          server_name example.com;
>          root /var/www/html;
>          index index.html;
>  
>          location / {
>              try_files $uri $uri/ =404;
>          }
>  
>          location /api {
>              proxy_pass http://backend;
>              proxy_set_header Host $host;
>          }
>  
>          location /static {
>              alias /var/www/static;
>              expires 30d;
>          }
>      }
>  
>      upstream backend {
>          server 192.168.1.101;
>          server 192.168.1.102;
>      }
>  }
>  ```
>  
>  ---
---

## **Summary of NGINX Components**
| **Component**         | **Role**                                                                 |
|------------------------|-------------------------------------------------------------------------|
| Master Process         | Controls worker processes and manages configuration.                    |
| Worker Processes       | Handle client requests.                                                 |
| Configuration File     | Defines NGINX behavior.                                                 |
| Modules                | Extend NGINX functionality.                                             |
| Upstream Blocks        | Define backend servers for load balancing or proxying.                  |
| Logs                   | Record access and error information.                                    |
| Cache                  | Improves performance by caching content.                                |
| SSL/TLS                | Handles secure communication.                                           |
| Variables              | Enable dynamic behavior in configurations.                              |
| Directives             | Configuration commands (e.g., `listen`, `server_name`, `location`).     |

---

# NGINX files and  directories

---

## **Default Files and Directories in `/etc/nginx/`**

Here’s a breakdown of the typical files and directories you’ll find in the `/etc/nginx/` folder:

---

## **1. `/etc/nginx/nginx.conf`**
- **Purpose:** The main configuration file for NGINX.  
- **Contents:**  
  - Global settings (e.g., `worker_processes`, `error_log`).  
  - Includes other configuration files (e.g., `conf.d/*.conf`, `sites-enabled/*`).  
- **Example:**  
  ```nginx
  user www-data;
  worker_processes auto;
  error_log /var/log/nginx/error.log warn;
  pid /var/run/nginx.pid;

  events {
      worker_connections 1024;
  }

  http {
      include /etc/nginx/mime.types;
      default_type application/octet-stream;
      include /etc/nginx/conf.d/*.conf;
      include /etc/nginx/sites-enabled/*;
  }
  ```

---

## **2. `/etc/nginx/conf.d/`**
- **Purpose:** Stores additional configuration files that are included in the main `nginx.conf`.  
- **Usage:**  
  - Ideal for global settings or configurations shared across multiple server blocks.  
  - Files in this directory typically have a `.conf` extension.  
- **Example File (`/etc/nginx/conf.d/gzip.conf`):**  
  ```nginx
  gzip on;
  gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
  ```

---

## **3. `/etc/nginx/sites-available/`**
- **Purpose:** Stores configuration files for individual server blocks (virtual hosts).  
- **Usage:**  
  - Each file represents a separate website or application.  
  - Files in this directory are not active unless symlinked to `/etc/nginx/sites-enabled/`.  
- **Example File (`/etc/nginx/sites-available/example.com.conf`):**  
  ```nginx
  server {
      listen 80;
      server_name example.com www.example.com;
      root /var/www/example.com;
      index index.html;
  }
  ```
> # Virtual host
>
> In NGINX, a **virtual host** (also known as a **server block**) is a
> configuration that allows you to host multiple websites or applications on a
> single server. Each virtual host is defined by a `server` block in the NGINX
> configuration and can have its own settings, such as domain name, root
> directory, and SSL certificates.
> 
> ### **What is a Virtual Host Used For?**
> Virtual hosts enable you to:
> 1. **Host Multiple Websites:** Serve different websites (e.g., `example.com` and `anotherexample.com`) from the same server.
> 2. **Isolate Configurations:** Use separate settings (e.g., root directories, SSL certificates) for each website.
> 3. **Optimize Performance:** Apply specific optimizations (e.g., caching, compression) for individual sites.
> 4. **Improve Security:** Restrict access or apply security measures (e.g., firewalls, authentication) per site.
> 
> ---
> 
> ### **How Virtual Hosts Work**
> - NGINX uses the `server_name` directive to determine which virtual host should handle an incoming request based on the domain name.
> - If no matching virtual host is found, NGINX uses the **default server block** (the first one defined or explicitly marked as default).
> 
> ---
> 
> ### **Basic Structure of a Virtual Host**
> A virtual host is defined within a `server` block in the NGINX configuration. Here’s an example:
> 
> ```nginx
> server {
>     listen 80;                          # Listen on port 80 (HTTP)
>     server_name example.com www.example.com;  # Domain names for this virtual host
> 
>     root /var/www/example.com;          # Root directory for the site's files
>     index index.html index.htm;         # Default files to serve
> 
>     location / {
>         try_files $uri $uri/ =404;      # Serve files or return 404 if not found
>     }
> }
> ```
> 
> ---
> 
> ### **Key Directives in a Virtual Host**
> | **Directive**       | **Description**                                                                 |
> |----------------------|---------------------------------------------------------------------------------|
> | `listen`            | Specifies the IP address and port to listen on (e.g., `listen 80;`).            |
> | `server_name`       | Defines the domain names for this virtual host (e.g., `server_name example.com;`). |
> | `root`              | Sets the root directory for the site’s files (e.g., `root /var/www/example.com;`). |
> | `index`             | Specifies the default files to serve (e.g., `index index.html;`).               |
> | `location`          | Configures how specific URLs are handled (e.g., `location / { ... }`).          |
> | `error_page`        | Defines custom error pages (e.g., `error_page 404 /404.html;`).                 |
> | `access_log`        | Specifies the log file for access logs (e.g., `access_log /var/log/example.com/access.log;`). |
> | `ssl_certificate`   | Specifies the SSL certificate for HTTPS (e.g., `ssl_certificate /etc/nginx/ssl/cert.pem;`). |
> | `ssl_certificate_key` | Specifies the SSL private key for HTTPS (e.g., `ssl_certificate_key /etc/nginx/ssl/key.pem;`). |
> 
> ---
> 
> ### **Example: Multiple Virtual Hosts**
> Here’s an example of two virtual hosts on the same server:
> 
> ```nginx
> # Virtual Host for example.com
> server {
>     listen 80;
>     server_name example.com www.example.com;
> 
>     root /var/www/example.com;
>     index index.html;
> 
>     location / {
>         try_files $uri $uri/ =404;
>     }
> }
> 
> # Virtual Host for anotherexample.com
> server {
>     listen 80;
>     server_name anotherexample.com www.anotherexample.com;
> 
>     root /var/www/anotherexample.com;
>     index index.html;
> 
>     location / {
>         try_files $uri $uri/ =404;
>     }
> }
> ```
> 
> ---
> 
> ### **Default Virtual Host**
> If no `server_name` matches the request, NGINX uses the **default server block**. You can explicitly define a default server block using the `default_server` parameter:
> 
> ```nginx
> server {
>     listen 80 default_server;
>     server_name _;  # Matches any domain name
> 
>     return 444;  # Close the connection without sending a response
> }
> ```
> 
> ---
> 
> ### **Virtual Hosts with SSL/TLS**
> To serve a site over HTTPS, add SSL/TLS directives to the virtual host:
> 
> ```nginx
> server {
>     listen 443 ssl;
>     server_name example.com www.example.com;
> 
>     ssl_certificate /etc/nginx/ssl/cert.pem;
>     ssl_certificate_key /etc/nginx/ssl/key.pem;
> 
>     root /var/www/example.com;
>     index index.html;
> 
>     location / {
>         try_files $uri $uri/ =404;
>     }
> }
> ```
> 
> ---
> 
> ### **Best Practices for Virtual Hosts**
> 1. **Use Separate Files:**  
>    Store each virtual host configuration in a separate file under `/etc/nginx/sites-available/` and symlink to `/etc/nginx/sites-enabled/`.  
> 2. **Use Descriptive Names:**  
>    Name configuration files after the domain (e.g., `example.com.conf`).  
> 3. **Test Configurations:**  
>    Always run `nginx -t` before reloading NGINX.  
> 4. **Enable HTTPS:**  
>    Use SSL/TLS for all sites to ensure secure communication.  
> 5. **Monitor Logs:**  
>    Check access and error logs for troubleshooting.  
> 
> ---
> 
> ### **Example Directory Structure**
> ```
> /etc/nginx/
> ├── sites-available/
> │   ├── example.com.conf
> │   └── anotherexample.com.conf
> ├── sites-enabled/
> │   ├── example.com.conf -> /etc/nginx/sites-available/example.com.conf
> │   └── anotherexample.com.conf -> /etc/nginx/sites-available/anotherexample.com.conf
> ```
> 
> ---

---

## **4. `/etc/nginx/sites-enabled/`**
- **Purpose:** Contains symlinks to the server block files in `/etc/nginx/sites-available/` that are currently active.  
- **Usage:**  
  - Enables or disables sites by creating or removing symlinks.  
  - Example of enabling a site:  
    ```bash
    sudo ln -s /etc/nginx/sites-available/example.com.conf /etc/nginx/sites-enabled/
    ```
  - Example of disabling a site:  
    ```bash
    sudo unlink /etc/nginx/sites-enabled/example.com.conf
    ```

---

## **5. `/etc/nginx/mime.types`**
- **Purpose:** Defines MIME types for file extensions.  
- **Usage:**  
  - Ensures NGINX serves files with the correct `Content-Type` header.  
  - Example:  
    ```nginx
    types {
        text/html html htm shtml;
        text/css css;
        application/javascript js;
    }
    ```

---

## **6. `/etc/nginx/modules-enabled/`**
- **Purpose:** Contains symlinks to enabled dynamic modules.  
- **Usage:**  
  - Modules are typically stored in `/usr/lib/nginx/modules/`.  
  - Example of enabling a module:  
    ```bash
    sudo ln -s /usr/lib/nginx/modules/ngx_http_geoip_module.so /etc/nginx/modules-enabled/
    ```

---

## **7. `/etc/nginx/ssl/`**
- **Purpose:** Stores SSL/TLS certificates and private keys.  
- **Usage:**  
  - Certificates and keys are referenced in server blocks for HTTPS.  
  - Example:  
    ```nginx
    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;
    ```

---

## **8. `/etc/nginx/streams-available/` and `/etc/nginx/streams-enabled/`**
- **Purpose:** Used for configuring TCP/UDP proxying (stream module).  
- **Usage:**  
  - Similar to `sites-available/` and `sites-enabled/`, but for stream configurations.  
  - Example File (`/etc/nginx/streams-available/example.conf`):  
    ```nginx
    stream {
        server {
            listen 12345;
            proxy_pass 192.168.1.101:54321;
        }
    }
    ```

---

## **9. `/var/log/nginx/`**
- **Purpose:** Stores NGINX log files.  
- **Files:**  
  - `access.log`: Logs all incoming requests.  
  - `error.log`: Logs errors and warnings.  
- **Usage:**  
  - Logs are essential for monitoring and debugging.  
  - Example configuration:  
    ```nginx
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log warn;
    ```

---

## **10. `/usr/share/nginx/html/`**
- **Purpose:** Default directory for serving static files.  
- **Usage:**  
  - This is the default `root` directory for the default server block.  
  - Example:  
    ```nginx
    server {
        listen 80;
        server_name _;
        root /usr/share/nginx/html;
        index index.html;
    }
    ```

---

## **11. `/etc/nginx/fastcgi.conf` and `/etc/nginx/fastcgi_params`**
- **Purpose:** Configuration files for FastCGI proxying (used with PHP, Python, etc.).  
- **Usage:**  
  - Defines parameters passed to FastCGI servers.  
  - Example:  
    ```nginx
    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
    ```

---

## **12. `/etc/nginx/proxy_params`**
- **Purpose:** Common proxy settings for reverse proxying.  
- **Usage:**  
  - Included in server blocks to simplify proxy configuration.  
  - Example:  
    ```nginx
    location / {
        include proxy_params;
        proxy_pass http://backend;
    }
    ```

---

## **Summary of `/etc/nginx/` Structure**
| **File/Directory**          | **Purpose**                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| `nginx.conf`                | Main configuration file.                                                   |
| `conf.d/`                   | Additional configuration files.                                            |
| `sites-available/`          | Configuration files for individual server blocks (not active by default).  |
| `sites-enabled/`            | Symlinks to active server block configurations.                            |
| `mime.types`                | Defines MIME types for file extensions.                                    |
| `modules-enabled/`          | Symlinks to enabled dynamic modules.                                       |
| `ssl/`                      | Stores SSL/TLS certificates and keys.                                      |
| `streams-available/`        | Configuration files for TCP/UDP proxying (not active by default).          |
| `streams-enabled/`          | Symlinks to active stream configurations.                                  |
| `/var/log/nginx/`           | Stores access and error logs.                                              |
| `/usr/share/nginx/html/`    | Default directory for serving static files.                                |
| `fastcgi.conf`              | Configuration for FastCGI proxying.                                        |
| `proxy_params`              | Common proxy settings for reverse proxying.                                |

---

### **Best Practices for Managing `/etc/nginx/`**
1. **Keep It Organized:**  
   Use `sites-available/` and `sites-enabled/` to manage server blocks.  
2. **Use Version Control:**  
   Track changes to your configuration files using Git.  
3. **Test Configurations:**  
   Always run `nginx -t` before reloading NGINX.  
4. **Backup Regularly:**  
   Backup your configuration files to avoid data loss.  
5. **Document Changes:**  
   Add comments to your configuration files to explain their purpose.  

---

# Best Practices and Guidlines

Handling NGINX configurations effectively is crucial for maintaining a secure,
performant, and manageable web server. Here are the **best practices** for
managing NGINX configurations:

---

### **1. Organize Configuration Files**
- **Use Modular Configuration:**  
  Split the configuration into multiple files for better organization. Use the `include` directive to include these files in the main configuration (`nginx.conf`).  
  Example:  
  ```nginx
  include /etc/nginx/conf.d/*.conf;
  include /etc/nginx/sites-enabled/*;
  ```
- **Separate Concerns:**  
  - Store global settings in `nginx.conf`.  
  - Store server blocks (virtual hosts) in separate files under `/etc/nginx/sites-available/`.  
  - Use symbolic links to enable sites in `/etc/nginx/sites-enabled/`.  
  Example:  
  ```bash
  sudo ln -s /etc/nginx/sites-available/example.com.conf /etc/nginx/sites-enabled/
  ```

---

### **2. Use Version Control**
- **Track Changes:**  
  Store your NGINX configuration files in a version control system (e.g., Git) to track changes, collaborate, and roll back if needed.  
- **Backup Configurations:**  
  Regularly back up your configuration files to avoid data loss.

---

### **3. Test Configurations Before Applying**
- **Check Syntax:**  
  Always test the configuration for syntax errors before reloading NGINX:  
  ```bash
  sudo nginx -t
  ```
- **Use Dry Runs:**  
  For complex changes, test them in a staging environment before deploying to production.

---

### **4. Optimize for Performance**
- **Tune Worker Processes:**  
  Set `worker_processes` to the number of CPU cores for optimal performance:  
  ```nginx
  worker_processes auto;
  ```
- **Increase Worker Connections:**  
  Adjust `worker_connections` based on expected traffic:  
  ```nginx
  events {
      worker_connections 1024;
  }
  ```
- **Enable Caching:**  
  Use `proxy_cache` to cache static and dynamic content, reducing backend load.  
- **Enable Gzip Compression:**  
  Compress responses to reduce bandwidth usage:  
  ```nginx
  gzip on;
  gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
  ```

---

### **5. Secure Your Configuration**
- **Disable Server Tokens:**  
  Hide NGINX version information from headers:  
  ```nginx
  server_tokens off;
  ```
- **Use Strong SSL/TLS Settings:**  
  Enable modern protocols and ciphers:  
  ```nginx
  ssl_protocols TLSv1.2 TLSv1.3;
  ssl_ciphers HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers on;
  ```
- **Restrict Access:**  
  Use `allow` and `deny` directives to control access to sensitive locations:  
  ```nginx
  location /admin {
      allow 192.168.1.0/24;
      deny all;
  }
  ```
- **Set File Permissions:**  
  Ensure configuration files are readable only by the root user:  
  ```bash
  sudo chmod 600 /etc/nginx/nginx.conf
  ```

---

### **6. Use Variables and Maps**
- **Dynamic Configuration:**  
  Use variables and `map` directives for dynamic behavior:  
  ```nginx
  map $http_user_agent $is_mobile {
      default 0;
      "~*android|iphone" 1;
  }
  server {
      if ($is_mobile) {
          return 301 /mobile;
      }
  }
  ```

---

### **7. Monitor and Log**
- **Enable Access and Error Logs:**  
  Use logs to monitor traffic and debug issues:  
  ```nginx
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log warn;
  ```
- **Custom Log Formats:**  
  Define custom log formats for better insights:  
  ```nginx
  log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';
  access_log /var/log/nginx/access.log main;
  ```

---

### **8. Automate Configuration Management**
- **Use Configuration Management Tools:**  
  Tools like Ansible, Puppet, or Chef can automate NGINX configuration deployment and ensure consistency across servers.  
- **CI/CD Pipelines:**  
  Integrate configuration testing and deployment into CI/CD pipelines for seamless updates.

---

### **9. Document Your Configuration**
- **Add Comments:**  
  Use comments to explain the purpose of each directive or block:  
  ```nginx
  # Redirect HTTP to HTTPS
  server {
      listen 80;
      server_name example.com;
      return 301 https://$host$request_uri;
  }
  ```
- **Maintain a README:**  
  Document the overall structure, key configurations, and any custom logic.

---

### **10. Regularly Review and Update**
- **Audit Configurations:**  
  Periodically review your configuration files to remove unused directives and optimize settings.  
- **Stay Updated:**  
  Keep NGINX and its modules up to date to benefit from security patches and new features.

---

### **Example Directory Structure**
```
/etc/nginx/
├── nginx.conf               # Main configuration file
├── conf.d/                  # Additional configuration files
├── sites-available/         # Available server blocks
│   └── example.com.conf
├── sites-enabled/           # Enabled server blocks (symlinks)
│   └── example.com.conf -> /etc/nginx/sites-available/example.com.conf
├── ssl/                     # SSL certificates
│   ├── cert.pem
│   └── key.pem
└── logs/                    # Log files
    ├── access.log
    └── error.log
```

---

# Real world scenarioes

Here are some **real-world scenarios** involving NGINX, along with questions and solutions to help you understand how to handle common situations:

---

## **Scenario 1: Hosting Multiple Websites on a Single Server**
**Question:**  
You have a single server and need to host two websites: `example.com` and `anotherexample.com`. How do you configure NGINX to serve both sites?

**Solution:**  
Use separate `server` blocks (virtual hosts) for each website:

```nginx
# Virtual Host for example.com
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# Virtual Host for anotherexample.com
server {
    listen 80;
    server_name anotherexample.com www.anotherexample.com;

    root /var/www/anotherexample.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

## **Scenario 2: Redirecting HTTP to HTTPS**
**Question:**  
You want to ensure all traffic to `example.com` is served over HTTPS. How do you configure NGINX to redirect HTTP to HTTPS?

**Solution:**  
Add a `server` block to redirect HTTP traffic and another for HTTPS:

```nginx
# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name example.com www.example.com;
    return 301 https://$host$request_uri;
}

# HTTPS Server Block
server {
    listen 443 ssl;
    server_name example.com www.example.com;

    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    root /var/www/example.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

## **Scenario 3: Load Balancing Across Multiple Backend Servers**
**Question:**  
You have three backend servers running your application, and you want NGINX to distribute traffic evenly among them. How do you set this up?

**Solution:**  
Use the `upstream` directive to define the backend servers and `proxy_pass` to forward requests:

```nginx
upstream backend {
    server 192.168.1.101;
    server 192.168.1.102;
    server 192.168.1.103;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

---

## **Scenario 4: Serving Static Files and Proxying Dynamic Requests**
**Question:**  
Your website serves static files (e.g., HTML, CSS, JS) and also has an API that runs on a backend server. How do you configure NGINX to handle both?

**Solution:**  
Use `location` blocks to serve static files and proxy API requests:

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example.com;
    index index.html;

    # Serve Static Files
    location / {
        try_files $uri $uri/ =404;
    }

    # Proxy API Requests
    location /api {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

---

## **Scenario 5: Enabling Caching for Static Content**
**Question:**  
You want to improve performance by caching static files (e.g., images, CSS, JS) for 30 days. How do you configure NGINX to do this?

**Solution:**  
Use the `expires` directive in the `location` block for static files:

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example.com;
    index index.html;

    # Cache Static Files for 30 Days
    location ~* \.(jpg|jpeg|png|gif|css|js|ico)$ {
        expires 30d;
        access_log off;
    }
}
```

---

## **Scenario 6: Restricting Access to an Admin Panel**
**Question:**  
You have an admin panel at `/admin` and want to restrict access to specific IP addresses. How do you configure NGINX to do this?

**Solution:**  
Use the `allow` and `deny` directives in the `location` block:

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example.com;
    index index.html;

    # Restrict Access to /admin
    location /admin {
        allow 192.168.1.0/24;  # Allow specific IP range
        deny all;               # Deny all other IPs
    }
}
```

---

## **Scenario 7: Serving a Single Page Application (SPA)**
**Question:**  
You have a Single Page Application (SPA) that uses client-side routing (e.g., React, Angular). How do you configure NGINX to serve the `index.html` file for all routes?

**Solution:**  
Use a `try_files` directive to serve `index.html` for all routes:

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example.com;
    index index.html;

    # Serve index.html for All Routes
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

---

## **Scenario 8: Handling WebSocket Connections**
**Question:**  
Your application uses WebSockets, and you want NGINX to proxy WebSocket connections to a backend server. How do you configure this?

**Solution:**  
Use the `proxy_pass` directive with WebSocket-specific headers:

```nginx
server {
    listen 80;
    server_name example.com;

    location /ws {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
    }
}
```

---

## **Scenario 9: Rate Limiting to Prevent Abuse**
**Question:**  
You want to limit the number of requests from a single IP address to prevent abuse. How do you configure NGINX to do this?

**Solution:**  
Use the `limit_req` directive to enforce rate limiting:

```nginx
http {
    limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;

    server {
        listen 80;
        server_name example.com;

        location / {
            limit_req zone=one burst=5;
            root /var/www/example.com;
            index index.html;
        }
    }
}
```

---

## **Scenario 10: Serving a Maintenance Page**
**Question:**  
You need to take your site offline for maintenance and serve a static maintenance page. How do you configure NGINX to do this?

**Solution:**  
Use the `return` directive to serve a maintenance page:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        return 503 /maintenance.html;
    }

    error_page 503 @maintenance;
    location @maintenance {
        root /var/www/example.com;
        rewrite ^(.*)$ /maintenance.html break;
    }
}
```

## **Scenario: Performing Maintenance on a Live Website**
**Question:**  
You need to take your website offline temporarily for maintenance. How do you configure NGINX to serve a maintenance page to all visitors while the site is being updated?

**Solution:**  
Use the `return` directive to serve a maintenance page and ensure all requests are redirected to it.

---

### **Step-by-Step Solution**

#### 1. Create a Maintenance Page
Create a simple HTML file for the maintenance page, e.g., `/var/www/maintenance.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Maintenance</title>
</head>
<body>
    <h1>Website Under Maintenance</h1>
    <p>We are currently performing maintenance. Please check back later.</p>
</body>
</html>
```

#### 2. Update the NGINX Configuration
Modify your NGINX configuration to serve the maintenance page for all requests:

```nginx
server {
    listen 80;
    server_name example.com;

    # Serve the maintenance page for all requests
    location / {
        return 503 /maintenance.html;
    }

    # Define the error page for 503 status
    error_page 503 @maintenance;
    location @maintenance {
        root /var/www;  # Directory where maintenance.html is located
        rewrite ^(.*)$ /maintenance.html break;
    }
}
```

#### 3. Reload NGINX
Test the configuration and reload NGINX:
```bash
sudo nginx -t  # Test the configuration
sudo systemctl reload nginx  # Reload NGINX
```

---

### **Explanation**
- **`return 503 /maintenance.html;`:**  
  Returns a 503 (Service Unavailable) status and serves the `maintenance.html` file for all requests.  
- **`error_page 503 @maintenance;`:**  
  Defines a custom error page for the 503 status.  
- **`location @maintenance { ... }`:**  
  Specifies the location block for serving the maintenance page.  
- **`rewrite ^(.*)$ /maintenance.html break;`:**  
  Ensures all requests are rewritten to serve the `maintenance.html` file.  

---

### **Additional Tips**
1. **Exclude Specific IPs:**  
   Allow access to the site for your team during maintenance by excluding specific IPs:  
   ```nginx
   location / {
       allow 192.168.1.0/24;  # Allow specific IP range
       deny all;               # Deny all other IPs
       return 503 /maintenance.html;
   }
   ```

2. **Use a Temporary Redirect:**  
   If the maintenance is brief, use a 302 (Temporary Redirect) instead of 503:  
   ```nginx
   location / {
       return 302 /maintenance.html;
   }
   ```

3. **Notify Search Engines:**  
   If the maintenance will last for an extended period, use a 503 status to inform search engines that the site will be back soon.  

---

### **Scenario: Restoring the Site After Maintenance**
**Question:**  
After completing maintenance, how do you restore the site to its normal configuration?

**Solution:**  
Revert the NGINX configuration to its original state and reload NGINX.

#### Steps:
1. Remove or comment out the maintenance configuration:  
   ```nginx
   # location / {
   #     return 503 /maintenance.html;
   # }
   # error_page 503 @maintenance;
   # location @maintenance {
   #     root /var/www;
   #     rewrite ^(.*)$ /maintenance.html break;
   # }
   ```

2. Reload NGINX:  
   ```bash
   sudo nginx -t  # Test the configuration
   sudo systemctl reload nginx  # Reload NGINX
   ```

---

### **Scenario: Monitoring During Maintenance**
**Question:**  
How do you monitor traffic and errors during maintenance to ensure everything is working as expected?

**Solution:**  
Use NGINX logs to monitor activity.

#### Steps:
1. Check the access log for requests:  
   ```bash
   sudo tail -f /var/log/nginx/access.log
   ```

2. Check the error log for issues:  
   ```bash
   sudo tail -f /var/log/nginx/error.log
   ```

---

### **Scenario: Rolling Back Changes**
**Question:**  
If something goes wrong during maintenance, how do you quickly roll back to the previous configuration?

**Solution:**  
Use version control or backups to restore the previous configuration.

#### Steps:
1. If using Git, revert to the previous commit:  
   ```bash
   git log  # Check the commit history
   git checkout <commit-hash>  # Revert to the previous commit
   ```

2. If using backups, restore the configuration file:  
   ```bash
   cp /etc/nginx/nginx.conf.backup /etc/nginx/nginx.conf
   ```

3. Reload NGINX:  
   ```bash
   sudo nginx -t
   sudo systemctl reload nginx
   ```

---

### **Summary of Maintenance Best Practices**
| **Task**                     | **Action**                                                                 |
|-------------------------------|---------------------------------------------------------------------------|
| **Serve a Maintenance Page**  | Use `return 503` and `error_page` to serve a custom maintenance page.     |
| **Exclude Specific IPs**      | Use `allow` and `deny` to allow access for your team.                     |
| **Monitor Logs**              | Check `access.log` and `error.log` for activity and issues.               |
| **Roll Back Changes**         | Use version control (e.g., Git) or backups to restore the configuration.  |
| **Notify Search Engines**     | Use a 503 status for extended maintenance.                                |

---

By following these steps, you can ensure a smooth maintenance process with minimal disruption to your users. Let me know if you’d like further details or additional scenarios! 🚀



---


# What is `root`?

The `root` directive in NGINX is used to **define the root directory** for files that are served by the server or location block. It specifies the base directory where NGINX looks for files to serve in response to requests. The `root` directive can appear in both the `server` block and the `location` block, and its behavior depends on where it is defined.

---

### **What Does `root` Do?**
- The `root` directive sets the **file system path** that NGINX uses to locate files for a given request.  
- When a request is made, NGINX appends the request URI to the `root` path to determine the file to serve.  
- For example, if the `root` is set to `/var/www/example.com` and a request is made for `/about.html`, NGINX will look for the file at `/var/www/example.com/about.html`.

---

### **`root` in the `server` Block**
When `root` is defined in the `server` block, it applies to **all locations** within that server block unless overridden by a `root` directive in a specific `location` block.

#### Example:
```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example.com;  # Root directory for the entire server block

    location / {
        index index.html;
    }

    location /images {
        # Files in /images are served from /var/www/example.com/images
    }
}
```
- In this example, all requests to `example.com` will look for files in `/var/www/example.com`.  
- A request to `example.com/about.html` will serve `/var/www/example.com/about.html`.  
- A request to `example.com/images/logo.png` will serve `/var/www/example.com/images/logo.png`.

---

### **`root` in the `location` Block**
When `root` is defined in a `location` block, it **overrides** the `root` directive in the `server` block for that specific location.

#### Example:
```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example.com;  # Default root for the server block

    location / {
        index index.html;
    }

    location /static {
        root /var/www/static;  # Override root for this location
    }
}
```
- Requests to `example.com/` will look for files in `/var/www/example.com`.  
- Requests to `example.com/static/style.css` will look for files in `/var/www/static/static/style.css` (note the double `static`).

---

### **Key Points About `root`**
1. **Appending Behavior:**  
   The `root` directive appends the request URI to the specified directory path.  
   Example:  
   - `root /var/www/example.com;` + `/about.html` → `/var/www/example.com/about.html`.

2. **Overriding:**  
   A `root` directive in a `location` block overrides the `root` directive in the `server` block for that location.

3. **Difference from `alias`:**  
   - `root` appends the request URI to the directory path.  
   - `alias` replaces the `location` path with the specified directory path.  
   Example:  
   ```nginx
   location /images {
       alias /var/www/images;
   }
   ```
   - A request to `/images/logo.png` will serve `/var/www/images/logo.png` (no `images` in the file path).

---

### **When to Use `root` vs `alias`**
- Use `root` when the request URI should be appended to the directory path.  
- Use `alias` when the request URI should be replaced by the directory path.

---

### **Example Scenarios**

#### Scenario 1: Serving Files from a Single Directory
```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example.com;

    location / {
        index index.html;
    }
}
```
- Request: `example.com/about.html` → File: `/var/www/example.com/about.html`.

#### Scenario 2: Serving Files from a Different Directory for a Specific Location
```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example.com;

    location /static {
        root /var/www/static;
    }
}
```
- Request: `example.com/static/style.css` → File: `/var/www/static/static/style.css`.

#### Scenario 3: Using `alias` to Serve Files
```nginx
server {
    listen 80;
    server_name example.com;

    location /images {
        alias /var/www/images;
    }
}
```
- Request: `example.com/images/logo.png` → File: `/var/www/images/logo.png`.

---

### **Best Practices**
1. **Use `root` for Most Cases:**  
   It’s simpler and more intuitive for most static file serving scenarios.  
2. **Use `alias` for Specific Paths:**  
   When you need to map a URL path to a different file system path without appending the location.  
3. **Avoid Duplicate Paths:**  
   Be careful with `root` in `location` blocks to avoid unintended double path segments (e.g., `/static/static`).  

---

# Practice

Here’s a **list of unanswered questions** to help you practice and deepen your understanding of NGINX. These questions cover a wide range of topics, from basic configuration to advanced use cases. Try to solve them on your own, and refer to the NGINX documentation or other resources if needed.

---

### **Basic Configuration**
1. How do you configure NGINX to serve a static HTML file located at `/var/www/html/index.html`?
2. How do you set up NGINX to listen on port 8080 instead of the default port 80?
3. How do you configure NGINX to serve a default page (e.g., `index.html`) when a directory is requested?
4. How do you enable directory listing in NGINX for a specific location?

---

### **Virtual Hosts**
5. How do you configure NGINX to host two websites (`example.com` and `anotherexample.com`) on the same server?
6. How do you set up a default server block to handle requests that don’t match any other server blocks?
7. How do you configure NGINX to redirect all traffic from `www.example.com` to `example.com`?

---

### **SSL/TLS**
8. How do you configure NGINX to serve a website over HTTPS using a self-signed certificate?
9. How do you enforce HTTPS for all traffic to your website?
10. How do you configure NGINX to use HTTP/2?

---

### **Reverse Proxy**
11. How do you configure NGINX to act as a reverse proxy for a backend server running on `localhost:3000`?
12. How do you pass the client’s IP address to the backend server when using NGINX as a reverse proxy?
13. How do you configure NGINX to cache responses from a backend server?

---

### **Load Balancing**
14. How do you configure NGINX to distribute traffic evenly across three backend servers (`192.168.1.101`, `192.168.1.102`, `192.168.1.103`)?
15. How do you configure NGINX to use the `least_conn` load balancing algorithm?
16. How do you configure NGINX to mark a backend server as down if it fails to respond?

---

### **Caching**
17. How do you configure NGINX to cache static files (e.g., images, CSS, JS) for 30 days?
18. How do you configure NGINX to cache dynamic content from a backend server?
19. How do you clear the NGINX cache?

---

### **Security**
20. How do you restrict access to a specific location (e.g., `/admin`) to a specific IP address?
21. How do you configure NGINX to prevent hotlinking of images?
22. How do you disable server tokens to hide the NGINX version in HTTP headers?

---

### **Performance Optimization**
23. How do you enable gzip compression in NGINX?
24. How do you configure NGINX to handle 10,000 simultaneous connections?
25. How do you configure NGINX to serve static files directly without passing them to a backend server?

---

### **Advanced Use Cases**
26. How do you configure NGINX to serve a Single Page Application (SPA) with client-side routing?
27. How do you configure NGINX to proxy WebSocket connections?
28. How do you configure NGINX to rate limit requests from a single IP address?
29. How do you configure NGINX to serve a maintenance page for all requests?
30. How do you configure NGINX to log requests in JSON format?

---

### **Debugging and Troubleshooting**
31. How do you test an NGINX configuration file for syntax errors?
32. How do you check the NGINX error log for issues?
33. How do you reload NGINX without dropping active connections?
34. How do you monitor the number of active connections in NGINX?

---

### **Miscellaneous**
35. How do you configure NGINX to serve a file for download instead of displaying it in the browser?
36. How do you configure NGINX to block requests from a specific user agent?
37. How do you configure NGINX to add custom HTTP headers to responses?
38. How do you configure NGINX to redirect all traffic from one domain to another?
39. How do you configure NGINX to serve a different file based on the client’s device (e.g., mobile vs. desktop)?
40. How do you configure NGINX to handle large file uploads?

---

### **Bonus Challenges**
41. How do you configure NGINX to act as a TCP/UDP load balancer?
42. How do you configure NGINX to serve a GeoIP-based redirect (e.g., redirect users from specific countries)?
43. How do you configure NGINX to integrate with Let’s Encrypt for automatic SSL certificate renewal?
44. How do you configure NGINX to serve a website with multiple languages (e.g., English and Spanish)?
45. How do you configure NGINX to serve a website with A/B testing?

---

### **How to Use These Questions**
1. **Set Up a Test Environment:**  
   Use a virtual machine or Docker container to practice NGINX configurations without affecting production systems.  
2. **Refer to Documentation:**  
   Use the [official NGINX documentation](https://nginx.org/en/docs/) as a reference.  
3. **Experiment and Test:**  
   Try different configurations and test them using tools like `curl` or a web browser.  
4. **Check Logs:**  
   Monitor NGINX logs (`access.log` and `error.log`) to debug issues.  

---

By working through these questions, you’ll gain hands-on experience with NGINX and become proficient in configuring it for various use cases. Let me know if you need help with any specific question! 🚀
