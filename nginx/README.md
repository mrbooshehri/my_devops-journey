
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
