# Install Teleport Ubuntu 20.04

1. Download the Teleport Repository GPG Signing Key

```bash
curl https://deb.releases.teleport.dev/teleport-pubkey.asc | sudo apt-key add -
```

2. Add Teleport Repository

```bash
add-apt-repository 'deb https://deb.releases.teleport.dev/ stable main'
```

3.  Update Apt Repository

```bash
apt update
```

4. Install Teleport 

```bash
apt-get install teleport
```

# Configure Teleport

1. Create teleport key home

```bash
mkdir -p /var/lib/teleport
```

2. Generate SSL Certificate

```bash
openssl req -x509 -nodes -newkey rsa:4096 \
-keyout /var/lib/teleport/teleport.key \
-out /var/lib/teleport/teleport.pem -sha256 -days 3650 \
-subj "/C=US/ST=Iran/L=NewYork/O=Tehran website/OU=Org/CN=example.com"
```

> **Note:** Replace `example.com` with yours

2. Generate Configuration File of Teleport

```bash
teleport configure -o /etc/teleport.yaml  \
--cluster-name=example.cluster \
--public-addr=teleport.example.com:443 \
--cert-file=/var/lib/teleport/teleport.pem \
--key-file=/var/lib/teleport/teleport.key
```

> **Note:** Replace `teleport.example.com` and `example.cluster` with yours

3. Check the configuration file

```bash
cat /etc/teleport.yaml
```

4. Start service

```bash
systemctl enable --now teleport
systemctl status teleport
```

5. create a new user and assign roles

```bash
tctl users add --roles=editor,access USERNAME
```

> **Note:** Replace `USERNAME` with yours

> **Note:** If your invitation code got expire run the following
command:

```bash

tctl users reset USERNAME
```


