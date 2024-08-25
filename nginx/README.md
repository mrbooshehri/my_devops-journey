
Table of Contents
=================

* [What is Nginx](#what-is-nginx)
* [Nginx vs HAProxy](#nginx-vs-haproxy)
* [Nginx installation](#nginx-installation)
   * [Ubuntu/Debian:](#ubuntudebian)
   * [RHEL/CentOS:](#rhelcentos)
   * [Configuration Files:](#configuration-files)
* [Key Files, Directories, and Commands](#key-files-directories-and-commands)
   * [Key Files and Directories:](#key-files-and-directories)
      * [Key Files:](#key-files)
      * [Key Directories:](#key-directories)
      * [Key Commands:](#key-commands)
* [Config file](#config-file)
   * [worker_connections](#worker_connections)
   * [use](#use)
   * [multi_accept](#multi_accept)
   * [accept_mutex](#accept_mutex)
   * [accept_mutex_delay](#accept_mutex_delay)
   * [debug_connection](#debug_connection)
   * [Breif summary of configuration file](#breif-summary-of-configuration-file)
* [Serving static contents](#serving-static-contents)
   * [1. Configuration](#1-configuration)
   * [2. Test Configuration](#2-test-configuration)
   * [3. Reload Nginx](#3-reload-nginx)
   * [Additional Tips](#additional-tips)
* [Graceful Reload](#graceful-reload)
* [HTTP Loadbalancing](#http-loadbalancing)
* [Additional Configuration](#additional-configuration)
* [TCP load balancing](#tcp-load-balancing)
* [Additional Configuration (Optional):](#additional-configuration-optional)
* [Tips](#tips)
   * [Shotdown nginx container](#shotdown-nginx-container)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->

I used this book as my reference, [link](https://www.amazon.com/NGINX-Cookbook-Advanced-High-Performance-Balancing/dp/1492078484) to but this book


![book.jpg](assets/book.jpg)

# What is Nginx

Nginx (pronounced "engine-x") is a popular open-source web server,
reverse proxy server, and load balancer. It is widely used to serve
static content, manage dynamic content, and act as a gateway for various
protocols.

Here are some key features and functionalities of Nginx:

1. **Web Server:** Nginx is primarily known as a high-performance web
	 server. It can handle a large number of concurrent connections
	 efficiently and serve static content (such as HTML, CSS, and images)
	 quickly.

2. **Reverse Proxy:** Nginx can act as a reverse proxy, forwarding
	 client requests to other servers, typically application servers like
	 those running applications built with technologies such as Node.js,
	 Python, Ruby, or PHP. This helps improve security, scalability, and
	 performance.

3. **Load Balancer:** Nginx can distribute incoming network traffic
	 across multiple servers to balance the load. This is especially
	 useful in high-traffic websites or applications to ensure that no
	 single server is overwhelmed.

4. **SSL/TLS Termination:** Nginx can handle SSL/TLS termination,
	 offloading the encryption and decryption process from the application
	 servers. This helps in reducing the workload on the backend servers
	 and improving overall performance.

5. **Caching:** Nginx includes caching mechanisms to store copies of
	 frequently requested static content. This helps reduce the load on
	 backend servers and speeds up content delivery.

6. **Security Features:** Nginx provides various security features,
	 including access control, rate limiting, and the ability to mitigate
	 certain types of DDoS attacks. It is designed to be robust and secure
	 in the face of potential threats.

7. **High Performance:** Nginx is designed for high performance and
	 efficiency. It is known for its low resource usage and ability to
	 handle a large number of simultaneous connections with minimal memory
	 footprint.

8. **Configuration Flexibility:** Nginx offers a flexible and powerful
	 configuration language, allowing administrators to fine-tune server
	 settings according to their specific requirements.


# Nginx vs HAProxy

Certainly! Let's compare Nginx (Open Source) with HAProxy (Community Edition) based on the information from the previous tables:

| Feature / Aspect               | Nginx (Open Source)                          | HAProxy (Community Edition)                   |
|---------------------------------|----------------------------------------------|----------------------------------------------|
Run with 'code -' to read from stdin (e.g. 'ps aux | grep code | code -').
| **Load Balancing**      | Basic load balancing capabilities                | Specialized, advanced load balancing        |
| **SSL/TLS Termination** | Yes                                              | Yes                                          |
| **Caching**             | Yes (for static content)                         | No (focused on load balancing)               |
| **TCP/UDP Load Balancing** | No                                            | Yes                                          |
| **Advanced Algorithms** | Limited                                          | Yes (supports a variety of algorithms)      |
| **Health Checking**     | Basic                                            | Sophisticated health-checking mechanisms    |
| **Configuration**       | Flexible and powerful                            | Tailored for load balancing, fine-grained   |
| **Use Cases**           | Web hosting, reverse proxy, SSL termination     | Load balancing for web/apps, TCP/UDP LB, HA  |
| **License**                     | Open-source (2-clause BSD-like license)     | GPL (GNU General Public License)             |
| **Support**                    | Community support                            | Community support                            |
| **Core Features**               | Web server, reverse proxy, basic load balancing, SSL/TLS termination, basic security features | Web server, reverse proxy, advanced load balancing, SSL/TLS termination, basic security features |
| **Load Balancing**              | Basic load balancing capabilities           | Advanced load balancing algorithms           |
| **Dynamic Reconfiguration**      | No                                           | Yes                                          |
| **Monitoring and Analytics**    | Limited monitoring capabilities              | Limited monitoring capabilities              |
| **Active Health Checks**        | No                                           | Yes                                          |
| **Configuration Reload**       | Yes                                          | Yes                                          |
| **Advanced Traffic Management** | Limited                                      | Traffic shaping, request rewriting            |
| **TCP/UDP Load Balancing**     | No (Primarily focused on HTTP)                | Yes                                          |
| **SSL/TLS Termination**        | Yes                                          | Yes                                          |
| **Cost**                        | Free (Open Source)                           | Free (Open Source)                           |


# Nginx installation

## Ubuntu/Debian:

1. Update the package lists:

```bash
sudo apt update
```

2. Install Nginx:

```bash
sudo apt install nginx
```

3. Start the Nginx service:

```bash
sudo systemctl start nginx
```

4. Enable Nginx to start on boot:

```bash
sudo systemctl enable nginx
```

## RHEL/CentOS:

1. Update the package lists:

```bash
sudo yum update
```

2. Install Nginx:

```bash
sudo yum install nginx
```

3. Start the Nginx service:

```bash
sudo systemctl start nginx
```

4. Enable Nginx to start on boot:

```bash
sudo systemctl enable nginx
```

## Configuration Files:

- Nginx configuration files are usually located in `/etc/nginx/`.
- The main configuration file is `/etc/nginx/nginx.conf`.
- Server block (similar to virtual host) configurations can be found in
	`/etc/nginx/sites-available/` (Ubuntu/Debian) or `/etc/nginx/conf.d/`
	(RHEL/CentOS).

# Key Files, Directories, and Commands

Nginx has several key files, directories, and commands that are
important for its configuration, operation, and management. Here's an
overview:

## Key Files and Directories:

Here's an overview of key files, directories, and commands associated
with Nginx:

### Key Files:

1. **nginx.conf:**
	 - Location: Usually in `/etc/nginx/nginx.conf` or
		 `/usr/local/nginx/conf/nginx.conf`.
	 - Purpose: The main configuration file for Nginx. It contains global
		 settings, server blocks, and various configuration directives.

2. **sites-available/ and sites-enabled/:**
   - Location: `/etc/nginx/sites-available/` and `/etc/nginx/sites-enabled/`.
	 - Purpose: Nginx allows you to organize server block configurations
		 in separate files within `sites-available/`. Files in
		 `sites-enabled/` are symbolic links to the configurations you want
		 to activate.

3. **mime.types:**
	 - Location: Inside the `conf/` directory, often in
		 `/etc/nginx/mime.types`.
	 - Purpose: Defines the mapping between file extensions and MIME
		 types. It helps Nginx determine how to handle different types of
		 files.

4. **fastcgi_params:**
	 - Location: Inside the `conf/` directory.
	 - Purpose: Contains common FastCGI parameters. It is often included
		 in FastCGI server block configurations.

5. **ssl_certificate and ssl_certificate_key:**
	 - Location: In SSL-enabled server block configurations.
	 - Purpose: These files contain the SSL certificate and its
		 corresponding private key, respectively, for enabling HTTPS.

### Key Directories:

1. **Main Configuration File:**
   - Default Path: `/etc/nginx/nginx.conf`
	 - This file contains the main configuration settings for Nginx. It
		 includes directives for global settings, worker processes, and
		 references to other configuration files.

2. **Server Block Configuration:**
   - Default Path (Ubuntu/Debian): `/etc/nginx/sites-available/`
   - Default Path (RHEL/CentOS): `/etc/nginx/conf.d/`
	 - Server block configuration files define settings for individual
		 websites or applications hosted on the server. Files in
		 `sites-available` (Ubuntu/Debian) or `conf.d` (RHEL/CentOS) are
		 typically symlinked to `sites-enabled` or included in the main
		 configuration file.

3. **Nginx Logs:**
   - Access Log: `/var/log/nginx/access.log`
   - Error Log: `/var/log/nginx/error.log`
	 - These logs provide information about requests, errors, and other
		 events related to Nginx. Log paths may vary based on the
		 configuration.

4. **Nginx PID File:**
   - Default Path: `/var/run/nginx.pid`
	 - The process ID (PID) file stores the process ID of the Nginx master
		 process. It is used for various administrative tasks, such as
		 reloading the configuration.

### Key Commands:

1. **Starting, Stopping, and Reloading Nginx:**
   - Start Nginx: `sudo service nginx start` or `sudo systemctl start nginx`.
   - Stop Nginx: `sudo service nginx stop` or `sudo systemctl stop nginx`.
   - Reload Nginx (without stopping it): `sudo service nginx reload` or `sudo systemctl reload nginx`.

2. **Testing Configuration:**
   - Check the syntax of the configuration file: `sudo nginx -t`.
   - If the syntax is correct, reload Nginx to apply changes: `sudo service nginx reload` or `sudo systemctl reload nginx`.

3. **Test Configuration**
	 - Purpose: The `nginx -T` command is used to test the syntax and
		 configuration of Nginx without actually applying the changes. It
		 performs a configuration test and then prints the parsed
		 configuration to the console.
	 - Usage: `sudo nginx -T`.

4. **Viewing Logs:**
   - Access log: `tail -f /var/log/nginx/access.log`.
   - Error log: `tail -f /var/log/nginx/error.log`.

5. **Version and Help:**
   - Check Nginx version: `nginx -v` or `nginx -V` for more details.
   - Get help: `nginx -h` or `man nginx`.

6. **Signal Command**
	 - Purpose: The `nginx -s` command is used to send signals to the
		 Nginx master process, allowing for various operations without
		 actually stopping and starting the server.
	 - Common Signals:
     - `nginx -s reload`: Reloads the configuration without stopping the server.
     - `nginx -s stop` or `nginx -s quit`: Gracefully stops the Nginx server.
     - `nginx -s reopen`: Reopens log files.

# Config file

The Nginx configuration file is organized into various sections, each
serving a specific purpose and defining settings for different aspects
of the web server. Here are some of the main sections found in an Nginx
configuration file:

1. **Main Context:**
   - **Syntax:** `main { ... }`
	 - The `main` context contains global configurations for the Nginx
		 server. It typically includes settings like the number of worker
		 processes, error log paths, and other general server-wide
		 configurations.

   Example:
   ```nginx
   user nginx;
   worker_processes auto;
   error_log /var/log/nginx/error.log;
   ```

	> 1. **user:**
	>    - **Syntax:** `user user_name [group_name];`
	> 	 - Specifies the user and group that Nginx worker processes should run
	> 		 as. It's a security measure to limit the permissions of worker
	> 		 processes.
	> 2. **worker_processes:**
	>    - **Syntax:** `worker_processes number;`
	> 	 - Defines the number of worker processes Nginx should spawn. Each
	> 		 worker process handles connections and requests independently. The
	> 		 `auto` keyword allows Nginx to automatically determine the number
	> 		 of CPUs available.
	> 3. **error_log:**
	>    - **Syntax:** `error_log file [level];`
	> 	 - Specifies the path to the error log file and the logging level. The
	> 		 logging levels include `debug`, `info`, `notice`, `warn`, `error`,
	> 		 and `crit`.
	> 4. **pid:**
	>    - **Syntax:** `pid file;`
	> 	 - Specifies the path to the file where the process ID (PID) of the
	> 		 main Nginx process will be stored.

2. **Events Context:**
   - **Syntax:** `events { ... }`
	 - The `events` context defines configurations related to event
		 processing, such as the maximum number of connections each worker
		 process can handle and other event-related settings.

   Example:
   ```nginx
   events {
       worker_connections 1024;
   }
   ```

	> ### `worker_connections`
	> 
	> - **Syntax:** `worker_connections number;`
	> - Specifies the maximum number of connections each worker process
	>    can handle simultaneously. It influences the overall capacity of
	>    the Nginx server.
	> 
	> - **Example:**
	>   ```nginx
	>   events {
	>       worker_connections 1024;
	>   }
	>   ```
	> 
	> ### `use`
	> 
	> - **Syntax:** `use [kqueue | rtsig | epoll | /path/to/dev/poll | select | poll];`
	> - Defines the event notification mechanism to be used. The
	>    appropriate mechanism depends on the operating system. Common
	>    values include `epoll` for Linux and `kqueue` for BSD systems.
	> 
	> - **Example:**
	>   ```nginx
	>   events {
	>       use epoll;
	>   }
	>   ```
	> 
	> ### `multi_accept`
	> 
	> - **Syntax:** `multi_accept on | off;`
	> - When set to `on`, allows each worker process to accept multiple
	>    connections at once, which can improve performance under high
	>    loads.
	> 
	> - **Example:**
	>   ```nginx
	>   events {
	>       multi_accept on;
	>   }
	>   ```
	> 
	> ### `accept_mutex`
	> 
	> - **Syntax:** `accept_mutex on | off;`
	> - Determines whether to enable or disable the accept mutex, which
	>    helps prevent thundering herd problems when multiple worker
	>    processes are trying to accept connections.
	> 
	> - **Example:**
	>   ```nginx
	>   events {
	>       accept_mutex on;
	>   }
	>   ```
	> 
	> ### `accept_mutex_delay`
	> 
	> - **Syntax:** `accept_mutex_delay time;`
	> - Specifies the time to wait before trying to acquire the accept
	>    mutex again. This helps to mitigate contention for the accept
	>    mutex.
	> 
	> - **Example:**
	>   ```nginx
	>   events {
	>       accept_mutex_delay 500ms;
	>   }
	>   ```
	> 
	> ### `debug_connection`
	> 
	> - **Syntax:** `debug_connection address;`
	> - Enables debugging information for connections from the specified
	>    IP address. Useful for debugging purposes.
	> 
	> - **Example:**
	>   ```nginx
	>   events {
	>       debug_connection 127.0.0.1;
	>   }
	>   ```

3. **HTTP Context:**
   - **Syntax:** `http { ... }`
   - The `http` context contains configurations related to the HTTP protocol. This includes server-wide HTTP settings, default types, access log settings, and the inclusion of `server` blocks.

   Example:
   ```nginx
   http {
       include /etc/nginx/mime.types;
       default_type application/octet-stream;

       log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                       '$status $body_bytes_sent "$http_referer" '
                       '"$http_user_agent" "$http_x_forwarded_for"';

       access_log /var/log/nginx/access.log main;
   }
   ```

4. **Server Context:**
   - **Syntax:** `server { ... }`
   - The `server` context defines configurations specific to a particular server block. Multiple `server` blocks can exist within the `http` context, allowing the server to host multiple sites or applications.

   Example:
   ```nginx
   server {
       listen 80;
       server_name example.com;

       location / {
           root /path/to/your/static/content;
           index index.html;
       }
   }
   ```

5. **Location Context:**
   - **Syntax:** `location { ... }`
   - The `location` context allows you to define configurations based on the location of a request. It is often used for setting specific rules for handling different types of requests or URIs.

   Example:
   ```nginx
   location /images/ {
       alias /path/to/your/image/directory/;
   }

   location ~ \.php$ {
       fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
       fastcgi_index index.php;
       include fastcgi_params;
       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
   }
   ```

## Breif summary of configuration file

| Section                  | Description                                                                          |
|--------------------------|--------------------------------------------------------------------------------------|
| `user`                   | Specifies the user and group for running Nginx processes.                              |
| `worker_processes`       | Sets the number of worker processes for handling connections.                          |
| `error_log`              | Defines the file for error logs.                                                      |
| `pid`                    | Specifies the file where the process ID of the main Nginx process is stored.          |
| `events`                 | Configures settings related to connection processing.                                 |
| `http`                   | Main HTTP configuration block containing server blocks, variables, and settings.     |
| `server`                 | Defines a virtual server block, including server_name, location blocks, and directives.|
| `location`               | Defines how to process requests based on their URI.                                   |
| `http` → `server` → `location` | Nested structure for specifying configurations within a specific location.         |
| `http` → `server`        | Configuration specific to a virtual server, including server-level directives.        |
| `http` → `upstream`      | Defines a group of servers that can be used in the `proxy_pass` directive.            |
| `http` → `types`         | Defines MIME types used to determine the content type of files.                        |
| `http` → `include`       | Includes external configuration files or snippets.                                    |
| `http` → `ssl_*`         | Configuration for SSL/TLS, including certificates, protocols, and ciphers.             |


# Serving static contents

Serving static content is one of the common use cases for web servers
like Nginx. Static content includes files like HTML, CSS, JavaScript,
images, and other assets that don't change dynamically on the server.
Here's a basic guide on how to serve static content using Nginx:


## 1. Configuration

The main Nginx configuration file is usually located at
`/etc/nginx/nginx.conf`. Here's a basic server block configuration for
serving static content:

```nginx
server {
    listen 80;
    server_name example.com;  # Replace with your domain or server IP

    location / {
        root /path/to/your/static/content;
        index index.html;
        try_files $uri $uri/ =404;
    }
}
```

- `listen`: Specifies the port on which Nginx will listen for incoming requests (in this case, port 80 for HTTP).
- `server_name`: Your domain name or server IP.
- `root`: The root directory where your static content is located.
- `index`: The default file to serve when a directory is accessed (e.g., `index.html`).
- `location`: Defines how Nginx processes requests. In this example, it tries to serve the requested file, and if not found, returns a 404 error.

> **Note:**
>
> In Nginx, the `default_server` directive is used within the `listen`
> directive to designate a server block as the default server for a
> specific IP address and port combination. This is particularly useful
> when Nginx is serving multiple virtual hosts (server blocks) on the
> same IP address and port, and a client makes a request without
> specifying a specific hostname that matches any of the configured
> server names.
> 
> ```nginx
> server {
>     listen 80 default_server;
>     server_name _;
> 
>     # Other server configurations for the default server...
> 
>     location / {
>         # Configuration for handling requests under the default server...
>     }
> }
> 
> server {
>     listen 80;
>     server_name example.com;
> 
>     # Other server configurations for example.com...
> 
>     location / {
>         # Configuration for handling requests under example.com...
>     }
> }
> ```
> 
> In this example:
> 
> - The first server block listens on port 80 and is marked as the
>   `default_server`.
> - `server_name _;` is used to match requests without a specific `Host`
>   header or requests with a `Host` header that doesn't match any
>   configured server names. The underscore (`_`) is a wildcard that
>   effectively catches any unmatched server names.
> - The second server block listens on port 80 for requests with the
>   `Host` header set to `example.com`.
> 
> When a request is made to the server without specifying a hostname
> (e.g., using the server's IP address or a domain that doesn't match
> any configured server names), Nginx routes the request to the default
> server, which is the one marked with `default_server`.
> 
> It's important to note that there can be only one `default_server` per
> IP address and port combination. If you have multiple server blocks
> using the same `listen` parameters, only one of them should have
> `default_server`. The `default_server` designation helps ensure that
> unmatched requests are directed to the intended server block.

## 2. Test Configuration

Before applying the changes, it's a good idea to test the configuration for syntax errors:

```bash
sudo nginx -t
```

## 3. Reload Nginx

After confirming that the configuration is correct, reload Nginx to apply the changes:

```bash
nginx -s reload
```

## Additional Tips

- **Optimizing Static Content:** Nginx is efficient at serving static
	files, but you can further optimize by enabling gzip compression,
	setting appropriate cache headers, and using a Content Delivery
	Network (CDN) if needed.

- **Security Considerations:** Ensure proper file permissions for your
	static content directory. Limit unnecessary access and consider using
	HTTPS for secure connections.


# Graceful Reload

Reloading the NGINX configuration without stopping the server provides
the ability to change configurations on the fly without dropping any
packets. In a high-uptime, dynamic environment, you will need to change
your load-balancing configuration at some point. NGINX allows you to do
this while keeping the load balancer online. This feature enables
countless possibilities, such as rerunning configuration management in a
live environment, or building an application and cluster-aware module
to dynamically configure and reload NGINX to meet the needs of the
environment.

```bash
nginx -s reload
```
# Loadbalancing

## HTTP Loadbalancing 

It's used when you need to distribute load between two or more HTTP
servers. 


Edit the Nginx configuration file (`/etc/nginx/nginx.conf` or a custom
file included in `nginx.conf`).

Add a new `http` block with `upstream` and `server` directives for load
balancing:

```nginx
http {
    upstream backend {
        server backend1.example.com weight=1;
        server backend2.example.com weight=2;
        server backend3.example.com backup;
        # Add more backend servers as needed
    }

    server {
        listen 80;

        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}
```

Explanation:

- The `upstream` module is used to define a group of servers
	that can be used to handle requests. This module is particularly
	useful for load balancing and proxying requests to a group of backend
	servers. The upstream block is used to declare a group of servers, and
	it can be referenced in various contexts within the Nginx
	configuration.
- The `server` directives within `upstream` specify individual backend
	servers.
- In the `server` block, the `location /` section configures the proxy
	to pass requests to the upstream backend.
- The `backup` server is used when the primary two are unavailable
- The `weight` instructs NGINX to pass twice as many requests to the
	second server, and the weight parameter defaults to 1.

	> Each upstream destination is defined in the upstream pool by the
	> server directive. The server directive is provided a Unix socket, IP
	> address, or a fully qualified domain name (FQDN), along with a
	> number of optional parameters.
	
	> FQDN is a complete domain name that specifies an exact location in
	> the hierarchical domain name system (DNS) tree. An FQDN includes the
	> top-level domain (TLD), the second-level domain (SLD), and any
	> subsequent subdomains, separated by dots. Each part of the domain is
	> read from right to left, with the more specific subdomains appearing
	> on the left.
	> 
	> The structure of an FQDN is as follows:
	> 
	> ```
	> subdomain2.subdomain1.domain.com.
	> ```
	> 
	> Here's a breakdown of the components:
	> 
	> - **subdomain2:** A specific subdomain under subdomain1.
	> - **subdomain1:** A subdomain under the main domain.
	> - **domain:** The main domain name.
	> - **com:** The top-level domain (TLD).
	> 
	> For example, in the FQDN `www.example.com.`, "www" is a subdomain,
	> "example" is the second-level domain, and "com" is the top-level
	> domain.
	> 
	> FQDNs are used to uniquely identify and locate resources on the
	> internet, such as websites, servers, and other networked services.
	> They are crucial for DNS to properly resolve and translate
	> human-readable domain names into IP addresses, allowing computers to
	> communicate over the internet.

	> # Additional Configuration
	>
	> - **Load Balancing Algorithms:**
	> 	- Nginx supports various load balancing algorithms such as
	> 		`round-robin` (default), `least_conn`, and `ip_hash`. You can
	> 		specify the algorithm in the `upstream` block. ```nginx
	>   upstream backend {
	>       least_conn;
	>       server backend1.example.com;
	>       server backend2.example.com;
	>   }
	>   ```
	> 
	> - **Health Checks:**
	> 	- Implement health checks to monitor the status of backend servers and
	> 		exclude unhealthy servers from the load balancing pool.
	>   ```nginx
	>   upstream backend {
	>       server backend1.example.com max_fails=3 fail_timeout=30s;
	>       server backend2.example.com max_fails=3 fail_timeout=30s;
	>   }
	>   ```
	> 
	> - **Session Persistence:**
	> 	- Use the `ip_hash` directive to implement session persistence based
	> 		on the client's IP address. 
	> 		```nginx
	>   	upstream backend {
	>     		ip_hash;
	>       	server backend1.example.com;
	>       	server backend2.example.com;
	>   	}
	>   	```

## TCP load balancing

TCP load balancing with Nginx is similar to HTTP load balancing, but it
is designed to handle generic TCP traffic, including protocols other
than HTTP.

Edit the Nginx configuration file (`/etc/nginx/nginx.conf` or a custom
file included in `nginx.conf`).

Add a new `stream` block with `upstream` and `server` directives for TCP
load balancing:

```nginx
stream {
    upstream backend {
        server backend1.example.com:1234;
        server backend2.example.com:1234;
        server backend3.example.com:1234;
        # Add more backend servers as needed
    }

    server {
        listen 1234;
        proxy_pass backend;
    }
}
```

Explanation:

- The `stream` block is used for TCP load balancing.
- The `upstream` block defines a group of backend servers with their IP
	addresses and port numbers.
- In the `server` block, the `listen` directive specifies the port on
	which Nginx will accept TCP connections.
- The `proxy_pass` directive forwards TCP connections to the defined
	backend servers.

## `http` vs `stream` module 

In Nginx, both the `http` and `stream` modules serve different purposes
and are designed for handling specific types of traffic. Let's compare
the `http` and `stream` modules in Nginx:

### `http` Module:

1. **Purpose:**
	 - The `http` module is designed to handle HTTP traffic, making it
		 suitable for web servers and applications that communicate using
		 the HTTP or HTTPS protocols.

2. **Use Cases:**
	 - It is used for serving web content, hosting websites, and
		 processing HTTP requests. It supports various features like
		 proxying, load balancing, SSL/TLS termination, and serving static
		 or dynamic content.

3. **Configuration Context:**
	 - The `http` module is configured in the main `nginx.conf` file or in
		 separate configuration files within the `conf.d/` or
		 `sites-available/` directories. It defines server blocks for
		 handling HTTP requests.

4. **Example Configuration:**
   ```nginx
   http {
       server {
           listen 80;
           server_name example.com;

           location / {
               # HTTP configurations...
           }
       }

       # Additional server blocks and configurations...
   }
   ```

### `stream` Module:

1. **Purpose:**
	 - The `stream` module is designed for handling generic TCP and UDP
		 traffic, making it suitable for scenarios like proxying, load
		 balancing, and TCP/UDP-based applications that do not use the HTTP
		 protocol.

2. **Use Cases:**
	 - It is used for scenarios where direct TCP or UDP communication is
		 required, such as proxying TCP connections to backend servers, load
		 balancing for non-HTTP protocols, or managing TCP/UDP-based
		 applications.

3. **Configuration Context:**
	 - The `stream` module is configured in the main `nginx.conf` file or
		 in separate configuration files within the `conf.d/` directory. It
		 defines server blocks for handling TCP or UDP connections.

4. **Example Configuration:**
   ```nginx
   stream {
       server {
           listen 443;
           proxy_pass backend_server;
           # TCP/UDP configurations...
       }

       # Additional server blocks and configurations...
   }
   ```

### Key Differences:

1. **Protocol Handling:**
	 - The `http` module is designed specifically for handling HTTP and
		 HTTPS traffic.
	 - The `stream` module is designed for handling generic TCP and UDP
		 traffic, not limited to HTTP.

2. **Configuration Context:**
	 - `http` configurations are used for web servers and are defined
		 within the `http` context in the main configuration file.
	 - `stream` configurations are used for handling TCP and UDP traffic
		 and are defined within the `stream` context in the main
		 configuration file.

3. **Use Cases:**
	 - Use the `http` module for web applications and services that rely
		 on the HTTP or HTTPS protocols.
	 - Use the `stream` module for scenarios where TCP or UDP traffic
		 needs to be proxied, load balanced, or managed.


**Breif demonstration**

| Feature                        | `http` Module                                      | `stream` Module                                    |
|--------------------------------|----------------------------------------------------|----------------------------------------------------|
| **Purpose**                    | Handles HTTP and HTTPS traffic.                   | Handles generic TCP and UDP traffic.               |
| **Use Cases**                  | Web servers, hosting websites, HTTP applications. | TCP and UDP scenarios, proxying, load balancing.   |
| **Configuration Context**      | Configured in `http` context in main `nginx.conf`. | Configured in `stream` context in main `nginx.conf`.|
| **Configuration File**         | `nginx.conf`, `sites-available/`, `conf.d/`.       | `nginx.conf`, `conf.d/`.                           |
| **Server Blocks**              | Defines server blocks for handling HTTP requests.  | Defines server blocks for handling TCP/UDP traffic.|
| **Protocol Handling**          | HTTP and HTTPS.                                   | TCP and UDP, not limited to HTTP.                  |


### TCP reverse proxy options

Here's a table with directives and options for configuring a reverse
proxy for TCP connections in Nginx using the `stream` module, along with
an example incorporating all of them:

```nginx
stream {
    server {
        listen 443;
        proxy_pass backend_server;
        proxy_bind 192.168.1.2;
        proxy_timeout 10s;
        proxy_buffer_size 4k;
        proxy_buffers 4 8k;
        proxy_ssl on;
        proxy_ssl_certificate /etc/nginx/ssl/proxy.crt;
        proxy_ssl_certificate_key /etc/nginx/ssl/proxy.key;
        proxy_protocol on;
    }

    server {
        listen 80;
        ssl_preread on;
        resolver 8.8.8.8;
        map $ssl_preread_server_name $backend {
            example.com backend_server1;
            default backend_server2;
        }
        proxy_pass $backend;
    }
}
```

| Directive                   | Description                                                        |
|-----------------------------|--------------------------------------------------------------------|
| `proxy_pass`                | Specifies the protocol and address of a proxied server.             |
| `proxy_bind`                | Binds a socket to a specific local address and port.               |
| `proxy_timeout`             | Sets the maximum time allowed for reading from or writing to the proxied server. |
| `proxy_buffer_size`         | Sets the size of the buffer used for reading from and writing to the proxied server. |
| `proxy_buffers`             | Sets the number and size of buffers used for reading from and writing to the proxied server. |
| `proxy_ssl`                 | Enables SSL for the proxied connection.                             |
| `proxy_ssl_certificate`     | Specifies the SSL certificate file for the proxied connection.     |
| `proxy_ssl_certificate_key` | Specifies the SSL certificate key file for the proxied connection. |
| `proxy_protocol`            | Enables or disables the use of the PROXY protocol for a connection. |
| `ssl_preread`               | Enables or disables the use of SSL preread for a connection.        |
| `resolver`                  | Defines the DNS resolver to use for dynamic backend resolution.     |
| `map`                       | Maps values based on conditions, used for dynamic backend selection. |

## UDP Load Balancing

UDP load balancing is used when You need to distribute load between two
or more UDP servers

Use NGINX’s stream module to load balance over UDP servers using the
upstream block defined as udp:
```nginx
stream {
	upstream ntp {
		server ntp1.example.com:123 weight=2;
		server ntp2.example.com:123;
	}
	server {
		listen 123 udp;
		proxy_pass ntp;
	}
}
```
This section of configuration balances load between two upstream network
time protocol (NTP) servers using the UDP protocol. Specifying UDP load
balancing is as simple as using the udp parameter on the listen
directive. If the service over which you’re load balancing requires
multiple packets to be sent back and forth between client and server,
you can specify the reuseport parameter. Examples of these types of
services are OpenVPN, Voice over Internet Protocol (VoIP), virtual
desktop solutions, and Datagram Transport Layer Security (DTLS). The
following is an example of using NGINX to handle OpenVPN connections and
proxy them to the OpenVPN service running locally:
```nginx
stream {
	server {
		listen 1195 udp reuseport;
		proxy_pass 127.0.0.1:1194;
	}
}
```
## Loadbalancing methods

Loadbalancing methods determine how incoming client requests are
distributed among a group of backend servers. Nginx provides several
load balancing algorithms or methods that you can use based on your
specific requirements. Here are some of the commonly used load balancing
methods in Nginx:

1. **Round Robin (`round-robin`):**
	 - The default method. Requests are distributed sequentially to each
		 server in turn. It evenly distributes the load across all backend
		 servers.

   ```nginx
   upstream backend_servers {
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
       round-robin;
   }
   ```

2. **Least Connections (`least_conn`):**
	 - Each request is sent to the server with the least number of active
		 connections. It is suitable for situations where backend servers
		 may have varying loads.

   ```nginx
   upstream backend_servers {
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
       least_conn;
   }
   ```

3. **IP Hash (`ip_hash`):**
	 - Assigns the client's IP address to one of the backend servers,
		 ensuring that requests from the same IP address always go to the
		 same server. Useful for maintaining session persistence.

   ```nginx
   upstream backend_servers {
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
       ip_hash;
   }
   ```

4. **Generic Hash (`hash`):**
	 - Assigns requests based on a user-defined key. It allows you to
		 direct requests with a specific attribute (e.g., a cookie value) to
		 the same backend server.

   ```nginx
   upstream backend_servers {
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
       hash $request_uri consistent;
   }
   ```

5. **Random (`random`):**
	 - Randomly selects a backend server for each request. It's a simple
		 method suitable for scenarios where true randomness is acceptable.

   ```nginx
   upstream backend_servers {
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
       random;
   }
   ```

6. **Least Time (`least_time`):**
	 - Chooses the server with the lowest average response time. It is
		 suitable for scenarios where backend servers may have varying
		 response times.

   ```nginx
   upstream backend_servers {
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
       least_time header;
   }
   ```

These load balancing methods can be applied within the `upstream` block
in the Nginx configuration file.


## Passive health checks

Passive health checks refer to a mechanism where the health of backend
servers is determined based on the responses received during regular
client requests, without actively probing or sending explicit health
check requests. In other words, Nginx assesses the health of backend
servers based on the success or failure of real client requests.

Here's how passive health checks generally work:

1. **Regular Client Requests:**
	 - Nginx continues to route regular client requests to the backend
		 servers as usual.

2. **Monitoring Responses:**
	 - Nginx monitors the responses received from the backend servers
		 during these regular client requests.

3. **Health Status Based on Responses:**
	 - The health of a backend server is determined based on the HTTP
		 status codes or other response characteristics. For example, a 2xx
		 or 3xx status might indicate a healthy server, while a 5xx status
		 might indicate a server issue.

4. **Automatic Adjustment:**
	 - Nginx automatically adjusts its routing decisions based on the
		 perceived health of the backend servers. Healthy servers receive
		 more requests, while unhealthy servers receive fewer or none.

Passive health checks are considered "passive" because the health of the
backend servers is assessed passively during normal traffic, and no
separate health-check requests are sent explicitly. This approach is
less intrusive and doesn't add extra load to the backend servers
specifically for health checks.

Nginx allows you to implement passive health checks through the use of
various configuration options, such as the `proxy_next_upstream`
directive, which controls the behavior when a request to a backend
server fails. For example:

```nginx

upstream backend {
	server backend1.example.com:1234 max_fails=3 fail_timeout=3s;
	server backend2.example.com:1234 max_fails=3 fail_timeout=3s;
}
```

This configuration passively monitors upstream health by monitoring the
response of client requests directed to the upstream server. The example
sets the max_fails directive to three, and fail_timeout to 3 seconds.
These directive parameters work the same way in both stream and HTTP
servers.


While passive health checks are simpler to implement, they may not be as
quick to detect backend server failures compared to active health
checks. Active health checks involve periodically sending dedicated
health-check requests to the backend servers, providing more timely
feedback about server health. The choice between passive and active
health checks depends on your specific use case and the trade-offs you
are willing to make in terms of responsiveness and server load. 

[More info](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-health-check/)


# Traffic Management

## A/B Testing

### What is A/B Testing

A/B testing, also known as split testing, is a method used in marketing
and product development to compare two versions of a webpage, email,
app, or other digital asset to determine which one performs better. The
goal is to identify changes that improve a specific metric or outcome,
such as click-through rates, conversion rates, or user engagement.

Here's how A/B testing typically works:

1. **Hypothesis:** Start with a hypothesis about a change that might
	 improve the performance of a webpage or element.

2. **Variations:** Create two or more versions (A and B) of the element
	 being tested, where one version (A) is the control, and the other
	 version (B) includes the proposed change.

3. **Randomization:** Randomly assign users or visitors to either
	 version A or version B. This helps ensure that any differences in
	 performance are due to the changes being tested rather than external
	 factors.

4. **Data Collection:** Measure and collect relevant data on the
	 performance of each version. This could include metrics like
	 click-through rates, conversion rates, or other key performance
	 indicators.

5. **Statistical Analysis:** Use statistical analysis to determine if
	 the differences in performance between the two versions are
	 statistically significant or if they could have occurred by chance.

6. **Decision:** Based on the results, decide whether to adopt the
	 changes made in version B or stick with the original version A.

A/B testing is widely used in online marketing and website optimization
to improve user experience, increase conversion rates, and optimize
various aspects of digital products. It allows businesses to make
data-driven decisions by testing different variations and understanding
the impact of changes on user behavior.


### Implementing A/B Testing with Nginx

Use the `split_clients` module to direct a percentage of your clients to a
different upstream pool:

```nginx
split_clients "${remote_addr}AAA" $variant {
	20.0% "backendv2";
	* "backendv1";
}
```

The `split_clients` directive hashes the string provided by you as the
first parameter and divides that hash by the percentages provided to map
the value of a variable provided as the second parameter. The addition
of “AAA” to the first parameter is to demonstrate that this is a
concatenated string that can include many variables, as mentioned in the
generic hash load-balancing algorithm. The third parameter is an object
containing key-value pairs where the key is the percentage weight and
the value is the value to be assigned. The key can be either a
percentage or an asterisk. The asterisk denotes the rest of the whole
after all percentages are taken. The value of the $variant variable will
be backendv2 for 20% of client IP addresses and backendv1 for the
remaining 80%. 

In this example, backendv1 and backendv2 represent upstream server pools
and can be used with the proxy_pass directive as such:

```nginx
location / {
	proxy_pass http://$variant
}
```

Using the variable $variant, our traffic will split between two
different application server pools.


To demonstrate the wide variety of uses split_clients can have, the following is an
example of splitting between two versions of a static site:
```nginx
http {
	split_clients "${remote_addr}" $site_root_folder {
		33.3% "/var/www/sitev2/";
		* "/var/www/sitev1/";
	}

	server {
	listen 80 _;
	root $site_root_folder;
	location / {
		index index.html;
		}
	}
}
```

[Official Documents](https://nginx.org/en/docs/http/ngx_http_split_clients_module.html)

### Using the GeoIP Module and Database

The GeoIP module in Nginx allows you to perform various actions based on
the client's geographical location. To use the GeoIP module, you need
the GeoIP database, which contains mapping information between IP
addresses and geographical locations.

#### Install GeoIP database

**Debian/Ubuntu systems:**
```bash
apt-get install nginx-module-geoip
```

**RHEL/CentOS systems:**
```bash
yum install nginx-module-geoip
```

**Download the GeoIP country and city databases and unzip them:**

```bash
mkdir /etc/nginx/geoip
cd /etc/nginx/geoip
wget "http://geolite.maxmind.com/download/geoip/database/GeoLiteCountry/GeoIP.dat.gz"
gunzip GeoIP.dat.gz
wget "http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz"
gunzip GeoLiteCity.dat.gz
```

With the GeoIP database for countries and cities on the local disk, you
can now instruct the NGINX GeoIP module to use them to expose embedded
variables based on the client IP address: 

```nginx
load_module "/usr/lib64/nginx/modules/ngx_http_geoip_module.so";
http {
	geoip_country /etc/nginx/geoip/GeoIP.dat; 
	geoip_city /etc/nginx/geoip/GeoLiteCity.dat;
# ...
}
```

The load_module directive dynamically loads the module from its path on
the filesys‐ tem. The load_module directive is only valid in the main
context. The geoip_country directive takes a path to the GeoIP.dat file
containing the database mapping IP addresses to country codes and is
valid only in the http context.

https://nginx.org/en/docs/http/ngx_http_geoip_module.html

### Restricting Access Based on Country

It used when you need to restrict access from particular countries for
contractual or application requirements.

Map the country codes you want to block or allow to a variable:

```nginx
load_module "/usr/lib64/nginx/modules/ngx_http_geoip_module.so";
http {
	map $geoip_country_code $country_access {
		"US" 0; 
		default 1;
	}
	# ...
}
```
This mapping will set a new variable $country_access to a 1 or a 0. If
the client IP address originates from the US, the variable will be set
to a 0. For any other country, the variable will be set to a 1. Now,
within our server block, we’ll use an if statement to deny access to
anyone not originating from the US:
```nginx
server {
	if ($country_access = '1') {
		return 403;
	}
	# ...
}
```
This if statement will evaluate True if the $country_access variable is
set to 1. When True, the server will return a 403 unauthorized.
Otherwise the server operates as normal. So this if block is only there
to deny people who are not from the US.

### Finding the Original Client

When Nginx acts as a reverse proxy, it receives requests from clients
and forwards them to backend servers. If you want to identify the
original client IP address when using Nginx as a reverse proxy, you need
to be aware of potential complications due to proxy headers.

Use the geoip_proxy directive to define your proxy IP address range and
the geoip_proxy_recursive directive to look for the original IP:
```nginx
load_module "/usr/lib64/nginx/modules/ngx_http_geoip_module.so";
http {
	geoip_country /etc/nginx/geoip/GeoIP.dat;
	geoip_city /etc/nginx/geoip/GeoLiteCity.dat;
	geoip_proxy 10.0.16.0/26;
	geoip_proxy_recursive on;
# ...
}
```
The geoip_proxy directive defines a classless inter-domain routing
(CIDR) range in which our proxy servers live and instructs NGINX to
utilize the X-Forwarded-For header to find the client IP address. The
geoip_proxy_recursive directive instructs NGINX to recursively look
through the X-Forwarded-For header for the last client IP known.

### Limiting Connections

You need to limit the number of connections based on a predefined key,
such as the client’s IP address


Construct a shared memory zone to hold connection metrics, and use the
`limit_conn` directive to limit open connections:

```nginx
http {
	limit_conn_zone $binary_remote_addr zone=limitbyaddr:10m;
	limit_conn_status 429;
	# ...
	server {
		# ...
		limit_conn limitbyaddr 40;
	# ...
	}
}
```
This configuration creates a shared memory zone named `limitbyaddr`. The
pre‐defined key used is the client’s IP address in binary form. The
size of the shared memory zone is set to 10 MB. The `limit_conn` directive
takes two parameters: a `limit_conn_zone` name and the number of
connections allowed. The `limit_conn_status` sets the response when the
connections are limited to a status of 429, indicating too many
requests. The `limit_conn` and `limit_conn_status` direc‐ tives are valid in
the http, server, and location context.


### Limiting Rate

You can limit the rate at which requests are processed using the
`limit_req` and `limit_req_zone` directives. These directives help
prevent abuse, control traffic, and protect your server from excessive
requests. Here's how you can configure rate limiting in Nginx:

1. **Install Nginx with Appropriate Modules:**

Ensure that your Nginx installation includes the necessary modules. You
may need to recompile Nginx with the `--with-http_limit_req_module`
option.

2. **Configure Rate Limiting:**

Edit your Nginx configuration file to include the rate limiting configurations. For example:

```nginx
http {
    limit_req_zone $binary_remote_addr zone=req_limit_per_ip:10m rate=5r/s;
		limit_req_status 429;

    server {
        listen 80;
        server_name example.com;

        location / {
            # Set a rate limit of 5 requests per second per IP
            limit_req zone=req_limit_per_ip burst=10 nodelay;

            # Your regular location configuration...
        }

        # Additional server configurations...
    }
}
```

In this example:

- `limit_req_zone $binary_remote_addr zone=req_limit_per_ip:10m
	rate=5r/s;`: This line creates a shared memory zone named
	`req_limit_per_ip` to store request limits based on the client's IP
	address. The rate is set to 5 requests per second.

- `limit_req zone=req_limit_per_ip burst=10 nodelay;`: This line applies
	the rate limit for the specified location. The `burst` parameter
	allows for a burst of up to 10 requests before the rate limit takes
	effect. The `nodelay` parameter ensures that requests exceeding the
	rate limit are immediately denied.


**Additional Considerations:**

- **Monitor and Adjust:**
	Regularly monitor your server's performance and adjust rate limits
	based on your observations. It's important to strike a balance between
	preventing abuse and allowing legitimate users to access your
	resources.

- **Customizing Error Responses:**
	When a client exceeds the rate limit, Nginx will return a `503 Service
	Unavailable` error. You can customize the error page or use the
	`limit_req_dry_run` directive to perform a dry run without actually
	limiting the requests.

```nginx
location / {
    limit_req zone=req_limit_per_ip burst=10 nodelay;
    limit_req_dry_run on;
    # Your regular location configuration...
}
```

This configuration will log warnings but won't actually limit requests,
allowing you to assess the impact before enforcing strict limits.

- **Limiting Overall Requests:** In addition to per-IP rate limits, you
	can set an overall rate limit for the entire server using
	`limit_req_zone $server_name zone=req_limit_per_server:10m
	rate=50r/s;` and `limit_req zone=req_limit_per_server burst=100
	nodelay;`, for example.


### Limiting Bandwidth

You can limit the bandwidth for requests or connections to control the
amount of data transferred. This is useful to prevent abuse, ensure fair
resource allocation, and manage server resources efficiently.

Utilize NGINX’s limit_rate and limit_rate_after directives to limit the
band width of response to a client:

```nginx
location /download/ {
	limit_rate_after 10m;
	limit_rate 1m;
}
```

The configuration of this location block specifies that for URIs with
the prefix `download`, the rate at which the response will be served to
the client will be limited after 10 MB to a rate of 1 MB per second. The
bandwidth limit is per connection, so you may want to institute a
connection limit as well as a bandwidth limit where applicable.


Limiting the bandwidth for particular connections enables NGINX to share
its upload bandwidth across all of the clients in a manner you specify.
These two directives do it all: `limit_rate_after` and `limit_rate`. The
limit_rate_after directive can be set in almost any context: `http`,
`server`, `location`, and `if` when the `if` is within a location. The
`limit_rate` directive is applicable in the same contexts as
`limit_rate_after`; however, it can alternatively be set by a variable
named `$limit_rate`. 

The `limit_rate_after` directive specifies that the
connection should not be rate limited until after a specified amount of
data has been transferred. The `limit_rate` directive specifies the rate
limit for a given context in bytes per second by default. However, you
can specify `m` for megabytes or `g` for gigabytes. Both directives default
to a value of 0. The value 0 means not to limit download rates at all.
This module allows you to programmatically change the rate limit of
clients.

# Massively Scalable Content Caching

Content caching boosts the speed of delivering content by saving
previous request responses for future use. By storing complete
responses, caching eliminates the need to recompute or re-query for the
same request, thereby reducing the workload on upstream servers. This
efficiency enhancement results in improved performance and decreased
resource usage, allowing for faster content delivery with fewer
resources. Strategically distributing caching servers can significantly
enhance user experience by hosting content closer to consumers,
optimizing performance. This approach aligns with the concept of content
delivery networks (CDNs), which cache content near users for better
performance. NGINX facilitates caching by enabling users to deploy
caching servers wherever NGINX can be placed, effectively allowing them
to create their own CDN. Additionally, NGINX caching allows for the
passive caching and serving of responses in case of an upstream server
failure. It's worth noting that caching features are only accessible
within the http context.

## Caching Zones

Caching zones are predefined memory areas used for storing
cached content and associated metadata. They allow Nginx to efficiently
manage cached objects and quickly serve cached responses to client
requests. Caching zones are defined using the `proxy_cache_path`
directive.

Here's an explanation of caching zones in Nginx:

1. **Definition and Configuration:**

You define caching zones using the `proxy_cache_path` directive in the
`http` block of your Nginx configuration file. This directive specifies
the path where cached content will be stored and configures various
parameters related to cache management.

```nginx
http {
    proxy_cache_path /path/to/cache levels=1:2 keys_zone=my_cache:10m max_size=10g inactive=60m use_temp_path=off;
}
```

- **`/path/to/cache`**: Specifies the directory where cached content will be stored on disk.

- **`levels=1:2`**: Sets the directory structure for cache storage. In
	this example, Nginx will create a two-level directory structure within
	the specified path.

- **`keys_zone=my_cache:10m`**: Defines a shared memory zone named
	`my_cache` with a size of 10 megabytes. This shared memory is used to
	store cache keys and metadata.

- **`max_size=10g`**: Specifies the maximum size of the cache storage on
	disk. In this example, the cache size is limited to 10 gigabytes.

- **`inactive=60m`**: Sets the maximum inactive time for cached content.
	Cached objects that have not been accessed within this time period
	will be considered stale and may be removed from the cache.

- **`use_temp_path=off`**: Specifies whether temporary files should be
	stored in a separate directory (`on`) or within the cache directory
	(`off`). Setting it to `off` can improve performance by reducing disk
	I/O.

2. **Keys Zone:**

The `keys_zone` parameter specifies the name and size of the shared
memory zone used to store cache keys and metadata. Each cached object is
associated with a unique key, which is used to quickly retrieve and
manage cached content.

3. **Cache Storage:**

Cached content is stored on disk within the specified cache directory
(`/path/to/cache` in the example). Nginx creates a hierarchical
directory structure based on the `levels` parameter to efficiently
manage a large number of cached objects.

4. **Cache Management:**

Nginx automatically manages cached content based on the configured
parameters such as maximum size (`max_size`), maximum inactive time
(`inactive`), and cache invalidation rules. Cached objects may be
removed or overwritten as needed to maintain cache size limits and
ensure cache freshness.

5. **Shared Memory:**

The shared memory zone (`keys_zone`) allows Nginx worker processes to
efficiently access and update cache metadata without the need for disk
I/O. This improves cache performance by reducing latency and resource
contention.

6. **Multiple Caching Zones:**

You can define multiple caching zones with different configurations to
cache content from different upstream servers or to apply different
caching policies based on the location or type of content.


## Cache Locking

Cache locking is a mechanism used to prevent cache stampedes or cache
contention issues that can occur when multiple requests attempt to
refresh the same cache item simultaneously. Cache locking ensures that
only one request is allowed to refresh a cache item at a time, while
other requests wait for the cache to be updated. This helps prevent
excessive load on the origin server and maintains cache consistency.

Nginx provides cache locking functionality through the
`proxy_cache_lock` directive, which allows you to control how cache
locking is implemented. Here's how cache locking works in Nginx:

1. **Configuration:**

You configure cache locking in the `http` block of your Nginx
configuration file using the `proxy_cache_lock` directive:

```nginx
http {
    proxy_cache_path /path/to/cache levels=1:2 keys_zone=my_cache:10m max_size=10g inactive=60m use_temp_path=off;

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_cache my_cache;
            proxy_cache_lock on;
            proxy_cache_lock_timeout 5s;
            proxy_cache_lock_age 10s;

            proxy_pass http://backend_server;
            # Other proxy and caching configurations...
        }
    }
}
```

- **`proxy_cache_lock`**: Enables or disables cache locking. When set to
	`on`, cache locking is enabled for the specified location.

- **`proxy_cache_lock_timeout`**: Specifies the maximum time a request
	will wait for the cache lock to be acquired. If the lock cannot be
	acquired within this timeout period, Nginx will serve the stale cached
	content (if available) or forward the request to the backend server.

- **`proxy_cache_lock_age`**: Specifies the minimum time a cache item
	must remain in the cache before it can be refreshed by another
	request. This helps prevent cache stampedes by ensuring that recently
	updated cache items are not refreshed too frequently.

2. **Cache Locking Process:**

- When a request attempts to refresh a cache item, Nginx checks if the
	cache lock is available.
- If the lock is available, Nginx acquires the lock and proceeds to
	refresh the cache item.
- If the lock is not available (i.e., another request is already
	refreshing the cache item), the requesting client waits for the lock
	to be released or for the `proxy_cache_lock_timeout` to expire.
- Once the cache item is refreshed, Nginx releases the lock, allowing
	other requests to refresh the same cache item.

3. **Benefits:**

- **Prevention of Cache Stampedes:** Cache locking prevents multiple
	requests from simultaneously refreshing the same cache item, reducing
	the load on the origin server and improving overall system stability.
  
- **Cache Consistency:** By ensuring that only one request updates a
	cache item at a time, cache locking helps maintain cache consistency
	and prevents race conditions.

**Summary:**

Cache locking in Nginx is a valuable feature for managing cache
contention and ensuring cache consistency in high-traffic environments.
By configuring cache locking parameters appropriately, you can optimize
cache performance and reduce the risk of cache-related issues such as
cache stampedes and race conditions.

## Caching Hash Keys

Caching hash keys are used to uniquely identify cached content and
determine where it should be stored in the cache storage. Hash keys are
generated based on specific request attributes, such as the request URI,
host header, or other variables. Properly configuring caching hash keys
ensures efficient caching and effective cache utilization. Here's how
caching hash keys work in Nginx:

(You need to control how your content is cached and retrieved.)


1. **Defining Cache Key:**

You define caching hash keys using the `proxy_cache_key` directive in
your Nginx configuration file. This directive specifies the variables
used to generate the cache key. For example:

```nginx
http {
    proxy_cache_path /path/to/cache levels=1:2 keys_zone=my_cache:10m max_size=10g inactive=60m use_temp_path=off;

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_cache my_cache;
            proxy_cache_key "$scheme$request_method$host$request_uri";
            # Other proxy and caching configurations...
        }
    }
}
```

In this example:

- **`proxy_cache_key`**: Specifies the variables used to generate the
	cache key. You can include any combination of variables, request
	attributes, or custom strings to form the cache key.

- **`$scheme$request_method$host$request_uri`**: This is an example of a
	cache key template. It includes variables such as the request scheme
	(`$scheme`), request method (`$request_method`), host header
	(`$host`), and request URI (`$request_uri`). You can customize the
	cache key template based on your caching requirements.

2. **Generating Cache Key:**

When a request is received, Nginx generates a cache key based on the
configured template and request attributes. The cache key uniquely
identifies the requested content and determines its location in the
cache storage.

3. **Caching Storage:**

Cached content is stored in the cache storage directory
(`/path/to/cache` in the example) based on the generated cache key.
Nginx creates a directory structure within the cache storage to
efficiently manage cached objects.

4. **Cache Lookup:**

When a subsequent request is received for the same content, Nginx
generates the cache key using the same template. It then looks up the
cache storage using the generated cache key to check if the requested
content is already cached.

5. **Benefits:**

- **Efficient Caching:** By using specific request attributes in the
	cache key, you can ensure that similar requests generate the same
	cache key and utilize the same cached content, improving cache hit
	rates and performance.

- **Customization:** You can customize the cache key template based on
	your caching requirements, including request attributes, variables,
	and custom strings.

- **Cache Invalidation:** Changes to the cache key template can
	effectively invalidate cached content or selectively purge cached
	objects based on specific criteria.


## Cache Bypass

Cache bypassing allows you to specify conditions under which caching
should be bypassed for certain requests. This is useful for scenarios
where you want to serve content directly from the origin server without
caching it, such as for dynamic content, authenticated requests, or
requests with specific query parameters. Nginx provides several
mechanisms for cache bypassing, including using the `proxy_cache_bypass`
directive or using `proxy_no_cache` and `proxy_cache_valid` directives
together. Here's how you can implement cache bypassing in Nginx:

1. Using `proxy_cache_bypass` Directive:

You can use the `proxy_cache_bypass` directive to conditionally bypass
caching based on request attributes. This directive allows you to define
conditions under which caching should be bypassed for a particular
request. For example:

```nginx
http {
    proxy_cache_path /path/to/cache levels=1:2 keys_zone=my_cache:10m max_size=10g inactive=60m use_temp_path=off;

    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_cache my_cache;
            proxy_cache_valid 200 302 10m;
            proxy_cache_valid 404 1m;

            # Bypass caching for requests with query parameters
            if ($args) {
                set $bypass_cache 1;
            }

            # Bypass caching based on custom condition
            if ($http_cookie ~* "session_id") {
                set $bypass_cache 1;
            }

            proxy_cache_bypass $bypass_cache;
            
            proxy_pass http://backend_server;
            # Other proxy and caching configurations...
        }
    }
}
```

In this example:

- **`proxy_cache_bypass`**: Specifies conditions under which caching
	should be bypassed. If the variable evaluates to true (non-zero),
	caching is bypassed for the request.

- **`$bypass_cache`**: This variable holds the condition for bypassing
	caching. It is set to 1 if certain conditions are met (e.g., requests
	with query parameters or specific cookies).

2. Using `proxy_no_cache` and `proxy_cache_valid` Directives Together:

Alternatively, you can use the combination of `proxy_no_cache` and
`proxy_cache_valid` directives to achieve cache bypassing. You set
`proxy_no_cache` to a condition and specify a short `proxy_cache_valid`
duration, effectively bypassing caching for requests that match the
condition. For example:

```nginx
location / {
    proxy_no_cache $cookie_session_id;
    proxy_cache_valid 200 302 1m;

    proxy_cache my_cache;
    proxy_pass http://backend_server;
    # Other proxy and caching configurations...
}
```

In this example, caching is bypassed for requests that contain a `session_id` cookie.


## Cache Performance

Cache performance in Nginx is crucial for improving the overall
responsiveness and scalability of web applications. Properly configuring
and managing caching can significantly reduce the load on backend
servers, decrease response times, and enhance the user experience. Here
are several factors that influence cache performance in Nginx:

1. Cache Hit Rate:

The cache hit rate represents the percentage of requests that are served
directly from the cache without needing to fetch content from the
backend server. A higher cache hit rate indicates better cache
performance and reduced reliance on backend resources.

2. Cache Size and Configuration:

The size and configuration of the cache storage affect its performance.
Ensure that the cache size is sufficient to accommodate frequently
accessed content and configure cache parameters such as `max_size`,
`inactive`, and `keys_zone` appropriately.

3. Cache Invalidation:

Proper cache invalidation mechanisms are essential for maintaining cache
consistency and ensuring that stale content is not served to users.
Implement cache invalidation strategies based on content expiration,
cache purging, or cache control headers to remove outdated content from
the cache.

4. Cache Key Optimization:

Optimizing cache keys based on request attributes, such as URLs, query
parameters, and headers, improves cache hit rates and enhances cache
performance. Use the `proxy_cache_key` directive to customize cache keys
and ensure efficient cache utilization.

5. Cache Bypassing:

Implement cache bypassing for dynamic or sensitive content that should
not be cached. Use directives like `proxy_no_cache` and
`proxy_cache_bypass` to selectively bypass caching based on request
attributes or conditions.

6. Cache Control Headers:

Use cache control headers such as `Cache-Control` and `Expires` to
instruct clients and intermediaries on how to cache content. Configure
Nginx to honor cache control headers and respect client-side caching
directives for optimal cache performance.

7. Monitoring and Tuning:

Regularly monitor cache performance metrics such as hit rate, cache
size, and cache utilization. Use tools like Nginx logging, monitoring
solutions, and performance testing to identify bottlenecks, adjust cache
configurations, and optimize cache performance accordingly.

8. CDN Integration:

Integrating Nginx with Content Delivery Networks (CDNs) can enhance
cache performance by distributing content closer to end users, reducing
latency, and offloading traffic from origin servers. Configure Nginx to
cache CDN responses and leverage edge caching for improved performance.

9. Load Balancing:

Implementing load balancing with Nginx distributes traffic across
multiple backend servers, reducing the load on individual servers and
enhancing cache performance. Use Nginx as a reverse proxy to balance
traffic and ensure optimal cache utilization across backend servers.

By addressing these factors and optimizing cache configurations, you can
maximize cache performance in Nginx, improve application scalability,
and deliver fast and responsive web experiences to users. Regular
monitoring, tuning, and experimentation are essential for continuously
optimizing cache performance and adapting to changing traffic patterns
and application requirements.

## Cache Slicing

It used when you need to increase caching efficiency by segmenting the file into fragments.

Use the NGINX `slice` directive and its embedded variables to divide the cache result
into fragments:

```nginx
proxy_cache_path /tmp/mycache keys_zone=mycache:10m;
server {
	# ...
	proxy_cache mycache;
	slice 1m;
	proxy_cache_key $host$uri$is_args$args$slice_range;
	proxy_set_header Range $slice_range;
	proxy_http_version 1.1;
	proxy_cache_valid 200 206 1h;
	location / {
		proxy_pass http://origin:80;
	}
}
```

This configuration defines a cache zone and enables it for the server.
The `slice` directive is then used to instruct NGINX to `slice` the response
into 1 MB file segments. The cache files are stored according to the
`proxy_cache_key` directive. Note the use of the embedded variable named
`slice_range`. That same variable is used as a header when making the
request to the origin, and that request HTTP version is upgraded to
HTTP/1.1 because 1.0 does not support byte-range requests. The cache
validity is set for response codes of 200 or 206 for 1 hour, and then
the location and origins are defined.

The Cache Slice module was developed for delivery of HTML5 video, which
uses byte-range requests to pseudostream content to the browser. By
default, NGINX is able to serve byte-range requests from its cache. If a
request for a byte range is made for uncached content, NGINX requests
the entire file from the origin. When you use the Cache Slice module,
NGINX requests only the necessary segments from the origin. Range
requests that are larger than the slice size, including the entire file,
trigger subrequests for each of the required segments, and then those
segments are cached. When all of the segments are cached, the response
is assembled and sent to the client, enabling NGINX to more efficiently
cache and serve content requested in ranges.

Cache Slice module should be used only on large files that do not
change. NGINX validates the ETag each time it receives a segment from
the origin. If the ETag on the origin changes, NGINX aborts the cache
population of the segment because the cache is no longer valid. If the
content does change and the file is smaller, or your origin can handle
load spikes during the cache fill process, it’s better to use the Cache
Lock module described in the blog listed in the following Also See
section. This module is not built by default, and needs to be enabled by
the `--with-http_slice_module` configuration when building NGINX.

# Programmability and Automation

## Using the NJS Module to Expose JavaScript Functionality Within NGINX 

The NJS (NGINX JavaScript) module allows you to extend NGINX
functionality by embedding JavaScript code directly within NGINX
configuration files. This enables you to leverage JavaScript to
customize request processing, implement complex logic, manipulate
variables, and interact with external systems—all within the NGINX
server or proxy environment. Here's an overview of using the NJS module
to expose JavaScript functionality within NGINX:

1. **Installation and Configuration:**
	 - Ensure that the NJS module is compiled and included in your NGINX
		 installation. NJS is typically available as a dynamic module that
		 can be added to NGINX during compilation.
	 - Configure NGINX to load the NJS module in the `nginx.conf` file
		 using the `load_module` directive.

2. **Embedding JavaScript Code:**
	 - Once the NJS module is loaded, you can embed JavaScript code
		 directly within NGINX configuration files using the `js_content`
		 directive or other NJS-specific directives.
	 - JavaScript code blocks can be placed within the NGINX `http`,
		 `server`, or `location` blocks, allowing you to define JavaScript
		 functions and logic at various levels of the server configuration.

3. **Defining JavaScript Functions:**
	 - Use JavaScript to define custom functions that perform specific
		 tasks or implement desired behavior within NGINX.
	 - JavaScript functions can manipulate request and response data,
		 modify NGINX variables, perform calculations, make external API
		 calls, and more.

4. **Interacting with NGINX Context:**
	 - JavaScript code executed within NGINX via the NJS module has access
		 to the NGINX request and response contexts, including request
		 headers, query parameters, and response status codes.
	 - You can access and modify NGINX variables, headers, and other
		 request/response attributes directly from JavaScript code.

5. **Handling Request Processing:**
	 - Use JavaScript to customize request processing logic within NGINX,
		 such as routing requests based on specific conditions, implementing
		 access control policies, or performing content transformation.
	 - JavaScript functions can be invoked at various stages of the
		 request processing pipeline, allowing for fine-grained control over
		 request handling.

6. **Implementing Dynamic Configurations:**
	 - JavaScript code executed within NGINX can dynamically generate
		 NGINX configurations based on runtime conditions, external data
		 sources, or configuration parameters.
	 - This enables flexible and adaptive configuration management,
		 allowing NGINX behavior to be adjusted dynamically based on
		 changing requirements or environmental factors.

7. **Extending NGINX Capabilities:**
	 - The NJS module opens up a wide range of possibilities for extending
		 NGINX capabilities using JavaScript, including implementing custom
		 load balancing algorithms, performing content manipulation or
		 transformation, enforcing security policies, and integrating with
		 external systems.

8. **Performance Considerations:**
	 - While JavaScript execution within NGINX via the NJS module
		 introduces additional processing overhead, it can provide
		 significant flexibility and extensibility benefits.
	 - Care should be taken to optimize JavaScript code for performance,
		 minimize blocking operations, and avoid excessive computational
		 overhead to ensure efficient request processing.

**Example:**

Here's a basic example demonstrating how to use the NJS module in NGINX
to expose JavaScript functionality:

```nginx
# Load the NJS module
load_module modules/ngx_http_js_module.so;

http {
    js_import mymodule.js;  # Import JavaScript file

    server {
        listen 80;
        server_name example.com;

        location / {
            js_content mymodule.processRequest;  # Execute JavaScript function for request processing
        }
    }
}
```

Now, let's create a JavaScript file named `mymodule.js` in the NGINX
configuration directory (usually `/etc/nginx`):

```javascript
// mymodule.js

function processRequest(r) {
    // Log the request details
    r.log("Request URL: " + r.uri);
    r.log("Client Address: " + r.remoteAddress);
    
    // Set response headers
    r.headersOut['Content-Type'] = 'text/plain';
    
    // Send custom response
    r.return(200, "Hello, NGINX with JavaScript!");
}
```

In this example:

- We load the NJS module using the `load_module` directive.
- We import a JavaScript file named `mymodule.js` using the `js_import`
	directive.
- Within the `server` block, we use the `js_content` directive to
	execute the `processRequest` function defined in the `mymodule.js`
	file for all requests to the specified location.
- The `processRequest` function logs request details, sets the response
	content type header, and returns a custom response with a "Hello,
	NGINX with JavaScript!" message.


# Extending NGINX with a Common Programming Language

Extending NGINX with a common programming language involves embedding
code written in that language within NGINX's configuration or source
code, enabling developers to customize NGINX behavior using familiar
programming paradigms. While NGINX itself is primarily written in C, it
supports various mechanisms for integrating code written in other
languages. Here's how you can extend NGINX with a common programming
language:

1. Lua:

Lua is a lightweight and embeddable scripting language that is commonly
used to extend NGINX's capabilities. The `ngx_lua` module allows you to
embed Lua code directly within NGINX configuration files or via Lua
modules, enabling dynamic content generation, request routing,
authentication, and more.

**Example:**
```nginx
location /hello {
    default_type 'text/plain';
    content_by_lua_block {
        ngx.say("Hello, NGINX with Lua!")
    }
}
```

2. JavaScript (NJS):

As discussed earlier, the NJS (NGINX JavaScript) module allows you to
embed JavaScript code directly within NGINX configuration files. This
enables you to leverage JavaScript for customizing request processing,
implementing complex logic, and interacting with NGINX contexts.

**Example:**
```nginx
js_import mymodule.js;

location /hello {
    js_content mymodule.processRequest;
}
```

3. Perl:

The `ngx_http_perl_module` allows you to embed Perl code within NGINX
configuration files, enabling advanced request processing, content
generation, and interaction with external systems. Perl scripts can be
executed directly within NGINX server blocks or via Perl modules.

**Example:**
```nginx
location /hello {
    perl 'sub { print "Hello, NGINX with Perl!\n"; }';
}
```

4. Python:

While NGINX does not natively support Python integration like Lua or
JavaScript, you can use third-party modules like
`ngx_http_python_module` or `mod_wsgi` in combination with NGINX to
execute Python code for dynamic content generation, request handling,
and application logic.

5. Other Languages:

There are various other ways to extend NGINX with common programming
languages:
- **FastCGI:** You can use NGINX as a reverse proxy to communicate with
	FastCGI applications written in languages like PHP, Python, or Ruby.
- **HTTP Modules:** NGINX supports HTTP modules written in C, allowing
	developers to extend NGINX's functionality directly within its source
	code.
- **Middleware Integration:** Middleware solutions like gRPC or Thrift
	can be integrated with NGINX to enable communication with applications
	written in different programming languages.

By extending NGINX with a common programming language, developers can
leverage their existing skills and tools to customize NGINX behavior,
implement complex logic, and build dynamic web applications with ease.
Each language has its own set of advantages and use cases, allowing
developers to choose the best tool for their specific requirements.


# Authentication

## HTTP Basic Authentication

Basic Authentication in NGINX is a simple method for restricting access
to web resources by requiring users to provide a username and password.
It's called "basic" because it sends user credentials as plaintext,
base64-encoded strings in the HTTP headers. While it's straightforward
to implement, it's not as secure as other methods like digest
authentication or token-based authentication. Here's how you can set up
basic authentication in NGINX:

1. Create an Authentication File:

First, you need to create a file that contains username-password pairs.
You can use the `htpasswd` utility to generate this file.

```bash
sudo htpasswd -c /etc/nginx/.htpasswd username
```

You'll be prompted to enter and confirm the password for the specified
username. Subsequent runs of `htpasswd` (without the `-c` flag) will add
additional users to the file.

2. Configure NGINX:

Once you have the authentication file, you can configure NGINX to use it
for authentication.

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/.htpasswd;
        # Other location configurations...
    }
}
```

- `auth_basic`: Enables basic authentication and sets the authentication
	realm, which is displayed to the user in the authentication prompt.
- `auth_basic_user_file`: Specifies the path to the authentication file
	created earlier.

### Example Use Case:

Suppose you want to restrict access to a specific directory, such as
`/admin`, on your website. You can add a location block for that
directory in your NGINX configuration:

```nginx
server {
    listen 80;
    server_name example.com;

    location /admin {
        auth_basic "Admin Area";
        auth_basic_user_file /etc/nginx/.htpasswd;
        # Other location configurations...
    }

    # Other server configurations...
}
```

Now, when users try to access any resource under `/admin`, they will be
prompted to enter a username and password. If the credentials match
those in the `.htpasswd` file, access will be granted; otherwise, they
will receive a 401 Unauthorized response.


##  Authentication Subrequests

Authentication subrequests in NGINX allow you to delegate authentication
to an external service or script, enabling flexible and dynamic
authentication mechanisms beyond simple username-password validation.
With authentication subrequests, NGINX forwards authentication requests
to an external authentication server or script, which then validates the
credentials and returns a response indicating whether the request should
be allowed or denied. Here's how authentication subrequests work in
NGINX:

1. Configuration Overview:

In NGINX, authentication subrequests are typically used in conjunction
with the `auth_request` directive. When a request is received by NGINX,
it forwards an authentication subrequest to a specified URI (internal or
external). The response from the authentication server determines
whether access to the requested resource is granted or denied.

### 2. Example Configuration:

```nginx
server {
    listen 80;
    server_name example.com;

    location /admin {
        auth_request /auth-proxy;
        error_page 401 = /auth-required;

        # Forward authentication subrequest to an authentication server
        proxy_pass http://auth-server;
    }

    location = /auth-proxy {
        internal;
        proxy_pass http://auth-server/auth;
        proxy_pass_request_body off;
        proxy_set_header Content-Length "";
        proxy_set_header X-Original-URI $request_uri;
    }

    location = /auth-required {
        return 401;
        # Optionally, you can include a WWW-Authenticate header with the type of authentication required
        # add_header WWW-Authenticate "Bearer realm=\"Restricted area\"";
    }

    # Other location configurations...
}
```

3. Configuration Breakdown:

- **`auth_request`**: Specifies the URI to which authentication
	subrequests should be forwarded. NGINX will include the original
	request details in the subrequest.
- **`error_page`**: Redirects requests that receive a 401 Unauthorized
	response to a specified location (e.g., `/auth-required`).
- **`proxy_pass`**: Forwards authentication subrequests to an external
	authentication server (`auth-server` in this example).
- **`proxy_pass_request_body`**: Configures whether to pass the request
	body to the authentication server.
- **`proxy_set_header`**: Sets additional headers for the authentication
	subrequest, such as `Content-Length` and `X-Original-URI`.
- **`internal`**: Marks the `/auth-proxy` location as internal,
	preventing direct external access.

4. Handling Authentication Response:

- If the authentication server returns a 200 OK response, NGINX allows
	access to the requested resource.
- If the authentication server returns a 401 Unauthorized response,
	NGINX redirects the user to the `/auth-required` location, where you
	can display a custom error page or provide further instructions.

# Security Controls

## Access Based on IP Address

Controlling access based on IP addresses in NGINX involves configuring
rules that allow or deny access to specific clients based on their IP
addresses. This can be useful for restricting access to certain
resources or securing administrative interfaces. Here's how you can
implement IP-based access control in NGINX:

1. Allow/Deny Access:

You can use the `allow` and `deny` directives within NGINX configuration
to control access based on IP addresses.

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        allow 192.168.1.0/24;   # Allow access from a specific IP range
        deny all;                # Deny access to all other IP addresses
        # Other location configurations...
    }

    # Other server configurations...
}
```

In this example:
- The `allow` directive permits access from the specified IP range
	(e.g., `192.168.1.0/24`).
- The `deny` directive blocks access from all other IP addresses.

2. Allow Specific IP Addresses:

To allow access only from specific IP addresses while denying access to
others, you can use multiple `allow` directives.

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        allow 192.168.1.100;   # Allow access from specific IP address
        allow 203.0.113.10;
        deny all;              # Deny access to all other IP addresses
        # Other location configurations...
    }

    # Other server configurations...
}
```

In this configuration, only requests from IP addresses `192.168.1.100`
and `203.0.113.10` will be allowed, while all others will be denied.

3. Handling IPv6 Addresses:

For IPv6 addresses, you can use similar syntax with the `allow` and
`deny` directives, specifying IPv6 addresses or ranges enclosed in
square brackets.

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        allow [2001:0db8::/32];  # Allow access from specific IPv6 range
        deny all;                # Deny access to all other IP addresses
        # Other location configurations...
    }

    # Other server configurations...
}
```

4. Handling Access to Specific Paths:

You can apply IP-based access control to specific paths or locations
within your NGINX configuration.

```nginx
location /admin {
    allow 192.168.1.0/24;   # Allow access to /admin only from specific IP range
    deny all;                # Deny access to /admin from all other IP addresses
    # Other configuration for /admin...
}
```

## Allowing Cross-Origin Resource Sharing

Allowing Cross-Origin Resource Sharing (CORS) in NGINX involves
configuring the server to include appropriate CORS headers in responses,
allowing web applications served from one domain to access resources
hosted on another domain. This is essential for enabling cross-origin
requests in web applications. Here's how you can implement CORS support
in NGINX:

1. Add CORS Headers:

You can use NGINX `add_header` directive to add CORS headers to
responses. These headers include `Access-Control-Allow-Origin`,
`Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`, etc.

```nginx
server {
    listen 80;
    server_name example.com;

    location /api {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type';
        # Other configuration for /api...
    }

    # Other server configurations...
}
```

In this example:
- `Access-Control-Allow-Origin` header allows requests from any origin
	(`*`). You can replace `*` with specific origins if needed.
- `Access-Control-Allow-Methods` header specifies the allowed HTTP
	methods (e.g., `GET`, `POST`, `OPTIONS`).
- `Access-Control-Allow-Headers` header lists the allowed request
	headers.

2. Handling Preflight Requests:

For some CORS requests, browsers first send a preflight request (HTTP
OPTIONS) to check the server's CORS policy. You need to handle these
requests and respond with appropriate CORS headers.

```nginx
location /api {
    if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' '*';
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
        add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type';
        add_header 'Content-Length' 0;
        add_header 'Content-Type' 'text/plain charset=UTF-8';
        return 204;
    }
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
    add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type';
    # Other configuration for /api...
}
```

In this configuration, if NGINX receives an OPTIONS request for the
`/api` location, it responds with appropriate CORS headers and a 204 No
Content status code.

3. Customize CORS Headers:

You can customize CORS headers based on your application's requirements.
For example, you can specify allowed origins, methods, headers, and
expose additional headers or credentials.

4. Testing:

After configuring CORS headers in NGINX, test your application to ensure
that cross-origin requests are allowed as expected.

## Client-Side Encryption

Using `ngx_http_ssl_module` or `ngx_stream_ssl_module` in NGINX
primarily involves configuring SSL/TLS encryption for HTTP or TCP/UDP
traffic, respectively. These modules enable NGINX to handle SSL/TLS
connections, providing encryption and security for data transmitted
between clients and servers. While these modules do not directly handle
client-side encryption (i.e., encryption performed by the client before
data transmission), they ensure that data is encrypted in transit,
enhancing overall security. Here's how you can use these modules to
implement SSL/TLS encryption in NGINX:

1. Configuring SSL/TLS for HTTP (ngx_http_ssl_module):

To enable SSL/TLS encryption for HTTP traffic in NGINX, you can use the
`ngx_http_ssl_module`. Here's a basic example of how to configure
SSL/TLS for an HTTP server block:

```nginx
http {
    server {
        listen 443 ssl;
        server_name example.com;

        ssl_certificate /path/to/certificate.crt;
        ssl_certificate_key /path/to/private.key;

        # Other HTTP server configurations...
    }
}
```

In this configuration:
- `listen 443 ssl;` specifies that NGINX should listen on port 443
	(default HTTPS port) and enable SSL/TLS encryption.
- `ssl_certificate` and `ssl_certificate_key` directives specify the
	paths to the SSL certificate and private key files, respectively.

2. Configuring SSL/TLS for TCP/UDP (ngx_stream_ssl_module):

To enable SSL/TLS encryption for TCP or UDP traffic (e.g., for protocols
like SMTP, POP3, IMAP, DNS, etc.), you can use the
`ngx_stream_ssl_module`. Here's a basic example of how to configure
SSL/TLS for a TCP stream block:

```nginx
stream {
    server {
        listen 443 ssl;
        proxy_pass backend_server;

        ssl_certificate /path/to/certificate.crt;
        ssl_certificate_key /path/to/private.key;

        # Other TCP stream configurations...
    }
}
```

In this configuration:
- `listen 443 ssl;` specifies that NGINX should listen on port 443
	(default HTTPS port) and enable SSL/TLS encryption.
- `proxy_pass` directive defines the backend server to which encrypted
	traffic will be forwarded.
- `ssl_certificate` and `ssl_certificate_key` directives specify the
	paths to the SSL certificate and private key files, respectively.


## Advanced Client-Side Encryption

Advanced client-side encryption in NGINX involves fine-tuning SSL/TLS
settings such as `ssl_protocols`, `ssl_ciphers`, `ssl_session_cache`,
and `ssl_session_timeout` to enhance security and performance. These
directives allow you to specify which SSL/TLS protocols and cipher
suites to use, as well as how to manage SSL/TLS session caching and
timeout. Here's how you can utilize these directives for advanced
client-side encryption in NGINX:

1. `ssl_protocols`:

The `ssl_protocols` directive specifies which SSL/TLS protocols are
allowed for secure connections. It allows you to control the protocol
versions supported by NGINX.

```nginx
ssl_protocols TLSv1.2 TLSv1.3;
```

In this configuration:
- Only TLS versions 1.2 and 1.3 are allowed for secure connections.

2. `ssl_ciphers`:

The `ssl_ciphers` directive specifies the cipher suites that NGINX will
use for encrypting data. It allows you to define the encryption
algorithms and key exchange mechanisms to be used.

```nginx
ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
```

In this configuration:
- NGINX will use specific cipher suites for encryption, prioritizing forward secrecy (e.g., `EECDH+AESGCM` and `EDH+AESGCM`).

3. `ssl_session_cache`:

The `ssl_session_cache` directive controls the caching of SSL/TLS
sessions, which helps reduce the overhead of establishing new SSL/TLS
connections.

```nginx
ssl_session_cache shared:SSL:10m;
```

In this configuration:
- NGINX will cache SSL/TLS sessions using a shared cache named `SSL`,
	with a maximum size of 10 megabytes.

4. `ssl_session_timeout`:

The `ssl_session_timeout` directive specifies the timeout for SSL/TLS
session caching. It defines how long cached SSL/TLS sessions remain
valid before expiring.

```nginx
ssl_session_timeout 5m;
```

In this configuration:
- Cached SSL/TLS sessions will expire after 5 minutes of inactivity.

Combined Configuration Example:

```nginx
server {
    listen 443 ssl;
    server_name example.com;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 5m;
    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;

    # Other server configurations...
}
```

In this combined configuration:
- NGINX listens on port 443 with SSL/TLS encryption enabled.
- Only TLS versions 1.2 and 1.3 are allowed for secure connections.
- Specific cipher suites are used for encryption, prioritizing forward
	secrecy.
- SSL/TLS session caching is enabled with a shared cache of 10 megabytes
	and a timeout of 5 minutes.
- SSL certificate and private key paths are specified for the server.


## Upstream encryption

Upstream encryption in NGINX involves securing communication between
NGINX and upstream servers, typically application servers or backend
services. This ensures that data transmitted between NGINX and upstream
servers is encrypted, providing confidentiality and integrity. Here's
how you can implement upstream encryption in NGINX:

1. HTTPS Configuration for Upstream Servers:

The most common approach to upstream encryption is to configure NGINX to
proxy requests to upstream servers over HTTPS. This involves configuring
NGINX to establish SSL/TLS connections with upstream servers.

```nginx
upstream backend {
    server backend1.example.com:443;
    server backend2.example.com:443;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass https://backend;
        # Other proxy configurations...
    }
}
```

In this configuration:
- `upstream` block defines backend servers (`backend1.example.com` and
	`backend2.example.com`) with port 443 (HTTPS).
- `proxy_pass` directive specifies that NGINX should proxy requests to
	the upstream servers over HTTPS.

2. SSL/TLS Certificate Verification:

To ensure the authenticity of upstream servers and prevent
man-in-the-middle attacks, you can configure NGINX to verify SSL/TLS
certificates presented by upstream servers.

```nginx
location / {
    proxy_pass https://backend;
    proxy_ssl_verify on;
    proxy_ssl_trusted_certificate /path/to/trusted_certificate.pem;
    # Other proxy configurations...
}
```

In this configuration:
- `proxy_ssl_verify` directive enables SSL/TLS certificate verification.
- `proxy_ssl_trusted_certificate` directive specifies the path to the
	trusted CA certificate file used for verification.

3. Client Certificate Authentication:

For additional security, you can configure NGINX to authenticate
upstream servers using client certificates. This ensures that only
authenticated clients (upstream servers) can establish connections with
NGINX.

```nginx
location / {
    proxy_pass https://backend;
    proxy_ssl_verify on;
    proxy_ssl_trusted_certificate /path/to/trusted_certificate.pem;
    proxy_ssl_client_certificate /path/to/client_certificate.pem;
    proxy_ssl_verify_depth 2;
    # Other proxy configurations...
}
```

In this configuration:
- `proxy_ssl_client_certificate` directive specifies the path to the
	client certificate file used for authentication.
- `proxy_ssl_verify_depth` directive specifies the maximum depth of the
	certificate verification chain.

4. Additional SSL/TLS Settings:

You can further customize SSL/TLS settings for upstream connections,
such as specifying allowed protocols, cipher suites, session caching,
and timeouts, similar to configuring SSL/TLS for client connections.

## Securing a Location

Using `secure_link_secret` in NGINX provides a method for creating
secure signed URLs. It allows you to generate URLs with an embedded
signature that verifies the authenticity of the request. Here's how you
can use `secure_link_secret` to secure a location in NGINX:

1. Generate a Secure Link:

First, generate a secure link with an embedded signature using the
`secure_link` directive.

```nginx
location /secure {
    secure_link $arg_signature,$arg_expires;
    secure_link_md5 "$secure_link_expires$uri$secure_link_secret";

    if ($secure_link = "") {
        return 403;
    }

    # Other secure location configurations...
}
```

In this configuration:
- The `secure_link` directive calculates a signature based on the
	provided parameters (`$arg_signature` and `$arg_expires`).
- The `secure_link_md5` directive generates an MD5 hash using the
	calculated signature, expiration time, URI, and secret key
	(`$secure_link_secret`).
- If the calculated signature does not match the provided signature
	(`$secure_link`), a 403 Forbidden response is returned.

2. Validate the Secure Link:

When a request is made to the secure location, NGINX calculates the
signature using the provided parameters and compares it with the
signature in the URL. If they match, access is granted; otherwise,
access is denied.

3. Configuration Example:

```nginx
server {
    listen 80;
    server_name example.com;

    location /secure {
        secure_link $arg_signature,$arg_expires;
        secure_link_md5 "$secure_link_expires$uri$secure_link_secret";

        if ($secure_link = "") {
            return 403;
        }

        # Other secure location configurations...
    }

    # Other server configurations...
}
```

In this example:
- Requests to the `/secure` location must include parameters `signature`
	and `expires`.
- The signature is calculated using the expiration time, URI, and secret
	key.
- If the calculated signature matches the provided signature, access is
	granted; otherwise, a 403 Forbidden response is returned.


## Securing a Location with an Expire Date

To secure a location in NGINX with an expiration date, you can use the
`secure_link` directive along with the `expires` parameter to generate
time-limited signed URLs. This approach ensures that access to the
protected location is granted only for a specified period. Here's how
you can implement this in NGINX:

1. **Configure NGINX:**

    First, configure NGINX to handle requests to the secured location and validate the signed URLs.

    ```nginx
    server {
        listen 80;
        server_name example.com;

        location /secure {
            secure_link $arg_md5,$arg_expires;
            secure_link_md5 "$secure_link_expires$uri$secure_link_secret";

            if ($secure_link = "") {
                return 403;
            }

            if ($secure_link_expires < $time_iso8601) {
                return 403;
            }

            # Other secure location configurations...
        }

        # Other server configurations...
    }
    ```

		- The `secure_link` directive calculates a signature based on the
			provided parameters (`$arg_md5` and `$arg_expires`).
		- The `secure_link_md5` directive generates an MD5 hash using the
			expiration time, URI, and secret key (`$secure_link_secret`).
		- The `$time_iso8601` variable represents the current time in ISO
			8601 format.
		- If the signature does not match or the expiration time has passed,
			NGINX returns a 403 Forbidden response.

2. **Generate a Secure Link:**

		In your application or script, generate a signed URL with the
		expiration date and MD5 checksum.

    ```bash
    # Set your secret key
    secret="your_secret_key"

    # Set expiration time (e.g., 1 hour from now)
    expires=$(date -d "1 hour" +%s)

    # Calculate MD5 checksum
    md5=$(echo -n "/secure${expires}${secret}" | md5sum | awk '{print $1}')

    # Construct the secure link
    secure_link="/secure?expires=${expires}&md5=${md5}"

    echo "Secure Link: http://example.com${secure_link}"
    ```

		This Bash script calculates the expiration time and MD5 checksum,
		then constructs the secure link with the appropriate parameters.

3. **Using the Secure Link:**

		When making a request to the secured location, include the generated
		parameters (expires and md5) in the URL.

    ```plaintext
    http://example.com/secure?expires=1643720400&md5=checksum_here
    ```

4. **Validation:**

		NGINX will validate the signed URL when a request is made to the
		secured location. If the signature matches and the expiration time
		has not passed, access is granted; otherwise, a 403 Forbidden
		response is returned.

This setup allows you to secure a location in NGINX with an expiration
date, ensuring that access to the protected resource is granted only for
a limited period.

## Generating an Expiring Link

To generate an expiring link in NGINX, you can use the `secure_link`
directive along with the `expires` parameter to create time-limited
signed URLs. These signed URLs include a signature that verifies their
authenticity and expiration time, allowing access only within a
specified period. Below is an example configuration demonstrating how to
achieve this in NGINX:

```nginx
server {
    listen 80;
    server_name example.com;

    location /secure {
        secure_link $arg_md5,$arg_expires;
        secure_link_md5 "$secure_link_expires$uri$secure_link_secret";

        if ($secure_link = "") {
            return 403;
        }

        if ($secure_link_expires < $time_iso8601) {
            return 403;
        }

        # Other secure location configurations...
    }

    # Other server configurations...
}
```

Explanation of the configuration:

- The `secure_link` directive calculates a signature based on the
	provided parameters (`$arg_md5` and `$arg_expires`).
- The `secure_link_md5` directive generates an MD5 hash using the
	expiration time, URI, and secret key (`$secure_link_secret`).
- If the signature does not match or the expiration time has passed,
	NGINX returns a 403 Forbidden response.

Now, to generate an expiring link, you would need to calculate the
expiration time and MD5 checksum using a programming language or script.
Below is an example using Bash scripting:

```bash
#!/bin/bash

# Set your secret key
secret="your_secret_key"

# Set expiration time (e.g., 1 hour from now)
expires=$(date -d "1 hour" +%s)

# Calculate MD5 checksum
md5=$(echo -n "/secure${expires}${secret}" | md5sum | awk '{print $1}')

# Construct the expiring link
expiring_link="/secure?expires=${expires}&md5=${md5}"

echo "Expiring Link: http://example.com${expiring_link}"
```

This script calculates the expiration time and MD5 checksum, then
constructs the expiring link with the appropriate parameters.

When using the expiring link in your application, make sure to include
the generated parameters (expires and md5) in the URL. For example:

```
http://example.com/secure?expires=1643720400&md5=checksum_here
```

When a request is made to the secured location (`/secure` in this case),
NGINX will validate the signed URL. If the signature matches and the
expiration time has not passed, access is granted; otherwise, a 403
Forbidden response is returned.

## HTTPS Redirects

Redirecting HTTP traffic to HTTPS in NGINX is a common practice to
ensure secure communication between clients and the server. You can
achieve this redirection using NGINX's `return` or `rewrite` directives
within the server block that listens on port 80. Here's how you can
implement HTTPS redirects in NGINX:

Using the `return` Directive:

```nginx
server {
    listen 80;
    server_name example.com;

    return 301 https://$server_name$request_uri;
}
```

In this configuration:
- NGINX listens for incoming HTTP requests on port 80.
- The `return` directive issues a permanent (301) redirect response to
	the corresponding HTTPS URL (`https://example.com$request_uri`).

Using the `rewrite` Directive:

```nginx
server {
    listen 80;
    server_name example.com;

    rewrite ^ https://$server_name$request_uri? permanent;
}
```

In this configuration:
- The `rewrite` directive performs a 301 permanent redirect to the HTTPS
	version of the same URL (`https://example.com$request_uri`).

Using a Separate Server Block for Redirects:

You can also use a separate server block to handle HTTP requests and
issue redirects:

```nginx
server {
    listen 80;
    server_name example.com;
    
    location / {
        return 301 https://$server_name$request_uri;
    }
}
```

In this configuration:
- The `location /` block captures all requests and issues a permanent
	redirect to the corresponding HTTPS URL.

Redirecting to HTTPS with WWW Prefix:

If you want to redirect both HTTP and non-WWW requests to HTTPS with the
WWW prefix, you can modify the redirect configuration as follows:

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    return 301 https://www.example.com$request_uri;
}
```

Redirecting with Custom Page:

You can also customize the redirect response by serving a custom HTML
page:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        return 301 https://$server_name$request_uri;
    }

    error_page 497 https://$server_name$request_uri;
}
```

In this case, when a client sends an HTTP request to the server, NGINX
responds with a 301 redirect status code and redirects the client to the
HTTPS version of the requested URL.

## Redirecting to HTTPS Where SSL/TLS Is Terminated Before NGINX

When SSL/TLS termination happens before NGINX, typically at a load
balancer or a reverse proxy like HAProxy, NGINX receives requests over
plain HTTP. In this scenario, NGINX needs to be configured to redirect
HTTP requests to HTTPS. Here's how you can achieve this redirection:

1. **Configure NGINX to Listen on Port 80:**
   
	 Ensure that NGINX is configured to listen for HTTP traffic on port
	 80:

   ```nginx
   server {
       listen 80;
       server_name example.com;
       
       # Other configurations...
   }
   ```

2. **Use `proxy_set_header` to Forward Protocol Information:**

	 Since NGINX receives requests over plain HTTP, it needs to be
	 informed that SSL termination has happened upstream. This can be
	 achieved by forwarding protocol information from the upstream load
	 balancer or reverse proxy. In NGINX, you can use the
	 `proxy_set_header` directive to set the `X-Forwarded-Proto` header:

   ```nginx
   server {
       listen 80;
       server_name example.com;

       # Forward protocol information from the load balancer
       proxy_set_header X-Forwarded-Proto $scheme;

       # Other configurations...
   }
   ```

3. **Use `map` Directive to Check for HTTPS:**

	 Use NGINX's `map` directive to check if the request came over HTTPS
	 or not based on the `X-Forwarded-Proto` header. If the request was
	 not over HTTPS, issue a redirect to the HTTPS version of the URL:

   ```nginx
   server {
       listen 80;
       server_name example.com;

       proxy_set_header X-Forwarded-Proto $scheme;

       # Check if the request came over HTTPS
       map $http_x_forwarded_proto $redirect_https {
           default "";
           "http" "https://$host$request_uri";
       }

       # Redirect to HTTPS if the request came over plain HTTP
       location / {
           return 301 $redirect_https;
       }
   }
   ```

   In this configuration:
	 - The `map` directive checks the value of the `X-Forwarded-Proto`
		 header.
	 - If the header indicates that the request came over plain HTTP,
		 NGINX constructs a redirect URL to the HTTPS version of the
		 requested URI.
	 - The `location /` block issues a 301 redirect to the constructed
		 HTTPS URL.

4. **Configure SSL/TLS in the HTTPS Server Block:**

	 Ensure that you have a separate server block configured to handle
	 HTTPS traffic. This server block should listen on port 443 and
	 contain your SSL/TLS certificate and key configurations.

   ```nginx
   server {
       listen 443 ssl;
       server_name example.com;

       ssl_certificate /path/to/certificate.crt;
       ssl_certificate_key /path/to/private.key;

       # Other SSL configurations...
   }
   ```

## HTTP Strict Transport Security 

HTTP Strict Transport Security (HSTS) is a security feature that
instructs web browsers to only connect to a website over HTTPS, even if
the user enters "http://" in the address bar. This helps prevent
man-in-the-middle attacks and ensures that all communication between the
browser and the server is encrypted. To enable HSTS in NGINX, you can
add the `add_header` directive to your server block. Here's how you can
implement HSTS in NGINX:

```nginx
server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;

    # Enable HSTS with a max-age of 365 days (one year)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    # Other server configurations...
}
```

In this configuration:

- The `add_header` directive adds the `Strict-Transport-Security` header
	to the server's response.
- `max-age=31536000` specifies that the HSTS policy should be enforced
	for one year (365 days).
- `includeSubDomains` instructs the browser to apply the HSTS policy to
	all subdomains of the current domain.
- `always` ensures that the header is included in all responses, not
	just successful ones.

##  Satisfying Any Number of Security Methods

In NGINX, you can implement multiple security measures to enhance the
protection of your web applications. To satisfy any number of security
methods, you can combine various directives and configurations within
your NGINX server blocks. Here's how you can implement a combination of
security methods in NGINX:

```nginx
server {
    listen 443 ssl;
    server_name example.com;

    ssl_certificate /path/to/certificate.crt;
    ssl_certificate_key /path/to/private.key;

    # Enable HSTS with a max-age of 365 days (one year)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    # Enable X-Content-Type-Options to prevent MIME sniffing
    add_header X-Content-Type-Options "nosniff";

    # Enable X-Frame-Options to prevent clickjacking attacks
    add_header X-Frame-Options "SAMEORIGIN";

    # Enable Content Security Policy (CSP) to mitigate XSS attacks
    add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline';";

    # Enable Referrer Policy to control referrer information
    add_header Referrer-Policy "strict-origin";

    # Enable feature policy to control APIs and browser features
    add_header Feature-Policy "accelerometer 'none'; camera 'none'; geolocation 'none';";

    # Other server configurations...
}
```

In this configuration:

- **HTTP Strict Transport Security (HSTS)**: Enforces the use of HTTPS
	by instructing browsers to only connect via HTTPS.
- **X-Content-Type-Options**: Prevents MIME type sniffing, reducing the
	risk of XSS attacks.
- **X-Frame-Options**: Protects against clickjacking attacks by
	restricting the use of the `<iframe>` element to the same origin.
- **Content Security Policy (CSP)**: Mitigates XSS attacks by specifying
	which resources can be loaded and executed on the page.
- **Referrer Policy**: Controls the amount of referrer information sent
	with requests, enhancing privacy and security.
- **Feature Policy**: Specifies which browser features and APIs can be
	used on the website, reducing the risk of unauthorized access to
	sensitive functionalities.

# HTTP/2


# Tips

## Shotdown nginx container

To shotdown an nginx container gracefully you should not kill the
container with `docker stop`, instead you should run the following
command:
```bash
docker exec <container-name> nginx -s quit
```

# [Udemy Nginx 2021- Beginner to Advanced](https://www.udemy.com/course/nginx-beginner-to-advanced/)

## 


# HAProxy vs Nginx (CE)

| Feature | HAProxy | Nginx |
| ------ | ------- | ------- |
| UDP Loadbalancing | ✗ | ✔ |
| Active health-check | ✔ | ✗ |
|  Slow start | ✔ | ✗ |
| Connection Dreaining | ✔ | ✗ |
| Sticky routing | ✔ | ✗ |
| Sticky cookie | ✔ | ✗ |
| Sticky Learn | ✔ | ✗ |
| | ✔ | ✗ |
| | ✔ | ✗ |
| | ✔ | ✗ |
| | ✔ | ✗ |
| | ✔ | ✗ |
| | ✔ | ✗ |
| | ✔ | ✗ |
| | ✔ | ✗ |
| | ✔ | ✗ |
