# How to install Confluence with postgress

## Postgress configuration
1. Create database user
```bash
su - postgres
createuser -S -d -r -P -E confuser
createdb --owner confuser --encoding utf8 confluence
```
**Note :** ```confuser``` and ```confluence``` are example names and you can change them however you want

2. Edit ```/var/lib/pgsql/11/data/pg_hba.conf``` file and add following line just after ```# IPv4 local connections:``` line
```bash
host    all             all             0.0.0.0/0               md5
```
3. Restart postgres service
```bash
systemctl restart postgres
```
## Download Confluence and atalassian-agent
1. Download Confluence Linux installer binary from [here](https://www.atlassian.com/software/confluence/download-archives)
1. Download ```atlassian-agent``` from [here](https://github.com/hgqapp/atlassian-agent)

## Install Confluence
Make the binary file that you have downloaded executable
```bash
chmod u+x atlasian-confluence-xxx.bin
```
then run it
```bash
./https://github.com/hgqapp/atlassian-agent
```
then follow the installation felow. Remember, **NOT TO START CONFLUENCE** during installation proccess or after it.

Now edit ```/PATH/TO/CONFLUENCE/bin/setenv.sh``` and add the the following line
```bash
export JAVA_OPTS="-javaagent:/PATH/TO//atlassian-agent.jar ${JAVA_OPTS}"
```
Now start confluence service
```bash
systemctl start confluenct
```
then open your browser and enter the IP address of your confluence machine, and follow the setup wizard.

In licence section copy ```server id```(you should do that via ```inspect element``` of your browser), the run the following commnad
```bash
java -jar /PATH/TO/atlassian-agent.jar  -m <EMAIL> -o <ORGANIZATION_NAME> -p conf -s <SERVER_ID>
```
**Note:** ```conf``` stands for confluence in the previous command.

Now copy the generated key and paste in in the relavant section in your browser and finish the setup wizard.
