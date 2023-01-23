# Install on Ubuntu/Debian

1. [Requirements](https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-requirements)
1. [Debian/Ubuntu server](https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-quickstart)
1. [Docker](https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-docker)
1. [Manual installation](https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-manual)

# Archive Messages on MySQL

1. Install MySQL
```bash
sudo apt install mysql-server
```
2. Change listen IP under `/etc/mysql/mysql.conf.d/mysqld.cnf`
```bash
bind-address = 127.0.0.1,<IP-ADDR>
```
3. Drop down to `mysql` and run the following queries:
```mysql
CREATE DATABASE prosody CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE USER 'prosody'@'%' IDENTIFIED BY '<strong-password>'; 
ALTER USER 'prosody'@'%' IDENTIFIED WITH mysql_native_password BY '<strong-password>';
GRANT ALL PRIVILEGES ON prosody.* TO 'prosody'@'%'; 
FLUSH PRIVILEGES;
```
4. Install the following packages:
```bash
apt install lua-dbi-mysql libssl-dev libffi-dev luarocks
luarocks install luajwtjitsi
```
5. Add the following configuration to `/etc/prosody/conf.d/example.com.cfg.lua`
	1. Adding `mod_muc_mam` [More info](https://prosody.im/doc/modules/mod_muc_mam)
	```bash
	Component "rooms.example.com" "muc"
			muc_log_by_default = true
			muc_log_presences = true
			log_all_rooms = ture
			muc_log_expires_after = "1w"
			muc_log_cleanup_interval = 4 * 60 * 60
			modules_enabled = { "mod_muc_mam"; }
	```
	2. Adding `mod_mam` [More info](https://prosody.im/doc/modules/mod_mam)
	```bash
	Component "conference.example.com" "muc"
			restrict_room_creation = true
			storage = "sql"
			sql = {
				driver = "MySQL"; -- May also be "MySQL" or "SQLite3" (case sensitive!)
				database = "prosody"; -- The database name to use. For SQLite3 this the database filename (relative to the data storage directory).
				host = "localhost"; -- The address of the database server (delete this line for Postgres)
				port = 3306; -- For databases connecting over TCP
				username = "prosody"; -- The username to authenticate to the database
				password = "VASLPass@123456"; -- The password to authenticate to the database
				}

			modules_enabled = {
					"muc_meeting_id";
					"muc_domain_mapper";
					"polls";
					"muc_mam";
					"websocket";
					--"token_verification";
					"muc_rate_limit";
			}
			admins = { "focus@auth.meet.vaslapp.com" }
			muc_room_locking = false
			muc_room_default_public_jids = true
			muc_room_default_persistent = true
			sql_manage_tables = true
	```
	
# Enable web socket 

1. Upgrade Prosody to 0.11.7
2. Update your Prosody config to enable websockets and smacks
In /etc/prosody/conf.avail/meet.domain.com.cfg.lua add websocket and smacks to modules_enabled.
Also add the websocket options: cross_domain_websocket and consider_websocket_secure to true:
```bash
       ...
       cross_domain_websocket = true;
       consider_websocket_secure = true;

        VirtualHost "meet.domain.com"
         ...
         modules_enabled = {
            "bosh";
            "websocket";
            "smacks";
            ...
         }

         smacks_max_unacked_stanzas = 5;
         smacks_hibernation_time = 60; 
         smacks_max_hibernated_sessions = 1;
         smacks_max_old_sessions = 1;
...
```
> **Note:** If you are using Jibri, make sure you enable `smaks` in that
`VirtualHost` section as well.

> **Note:** [smakcs installation guid](https://modules.prosody.im/mod_smacks)

3. Update your nginx config to map xmpp-websocket

In `/etc/nginx/sites-available/meet.domain.com.conf` add a section to
map the location `/xmpp-websocket.` 

> **Note:** This should match BOSH. If /http-bind points to 5280 then /xmpp-websocket should too:

> **Update:** Add the `x-jitsi-shard` and `Access-Control-Expose-Headers` headers to avoid the `Detected that shard changed from ... to null` error

```bash
# ...

server 
{
    listen meet.domain.com:443 ssl http2;
    server_name meet.domain.com;

# ...

    location = /xmpp-websocket
    {
        proxy_pass http://localhost:5280/xmpp-websocket;
       
        #shard & region that matches config.deploymentInfo.shard/region -  See [note 1] below
        add_header 'x-jitsi-shard' 'shard';
        add_header 'x-jitsi-region' 'us-east-2a';
        add_header 'Access-Control-Expose-Headers' 'X-Jitsi-Shard, X-Jitsi-Region';

        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host $http_host;

        proxy_http_version 1.1;
        proxy_buffer_size 128k;
        proxy_buffers 4 256k;
        proxy_busy_buffers_size  256k;
        proxy_set_header Connection "upgrade";
        tcp_nodelay on;
    }
# ...
```

4. Enable Websockets in your config.js

Edit your `/etc/jitsi/meet/meet.domain.com-config.js` and uncomment the `Websocket` URL:

```bash
...
     // BOSH URL
    bosh: '//meet.domain.com/http-bind',

    // Websocket URL
    websocket: 'wss://meet.domain.com/xmpp-websocket',
...
```

5. Restart Services

```bash
service nginx restart && service prosody restart && service jicofo restart
```

## Useful links

1. [how to enable websockets xmpp websocket and smacks for prosody](https://community.jitsi.org/t/how-to-how-to-enable-websockets-xmpp-websocket-and-smacks-for-prosody/87920)
1. [State of XMPP connection : Bosh or websocket](https://community.jitsi.org/t/state-of-xmpp-connection-bosh-or-websocket/79721)
1. [configure xmmp framework to support both websocket and BOSH](https://community.jitsi.org/t/jitsi-dev-configure-xmmp-framework-to-support-both-websocket-and-bosh/10258)
1. [WebSockets – Prosody IM](https://prosody.im/doc/websocket)

# Enable authentication


# Handle specific errors

1. If you face with one of the following errors
	1. `Error in SQL transaction: /usr/share/lua/5.2/prosody/util/sql.lua:151: Error executing statement parameters: Incorrect arguments to mysqld_stmt_execute`
	1. `conference.meet.vaslapp.com:muc_mam     error   Could not fetch history: /usr/share/lua/5.2/prosody/util/sql.lua:151: Error executing statement parameters: Incorrect arguments to mysqld_stmt_execute`
edit `function archive_store:find` section in `mod_storage_sql.lua`
file.
```lua
--              if query.limit then
--                      args[#args+1] = query.limit;
--              end

--              sql_query = sql_query:format(t_concat(where, " AND "), query.reverse
--                      and "DESC" or "ASC", query.limit and " LIMIT ?" or "");
                sql_query = sql_query:format(t_concat(where, " AND "), query.reverse
                        and "DESC" or "ASC", query.limit and " LIMIT " .. query.limit or "");

```
for more info check 15th answer [here](https://issues.prosody.im/1639)
