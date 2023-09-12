# Setup PPTP client cli

1. Update system

```bash
apt-get update
```

2. Install PPTP Client

```bash
apt install pptp-linux
```

3. Create connection configuration

```bash
vim /etc/ppp/peers/CUSTOM_CONFIG_NAME
```

4. Put the following config in it

```bash
pty "pptp Your_Server_IP --nolaunchpppd --debug"
name Your_Username
password Your_Password
remotename PPTP
require-mppe-128
require-mschap-v2
refuse-eap
refuse-pap
refuse-chap
refuse-mschap
noauth
debug
persist
maxfail 0
defaultroute
replacedefaultroute
usepeerdns
```

5. Change the PPTP file permission

```bash
chmod 600 /etc/ppp/peers/CUSTOM_CONFIG_NAME
```

6. Connecting and disconnecting VPN

	1. To connect

	```bash
	pon CUSTOM_CONFIG_NAME
	```

	1. To disconnect

	```bash
	poff CUSTOM_CONFIG_NAME
	```
