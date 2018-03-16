# docker ServiceNow MID Server

This is a Dockerfile to set up a "ServiceNow MID Server" - (https://docs.servicenow.com/bundle/kingston-servicenow-platform/page/product/mid-server/reference/r-MIDServer.html#)

## Build from docker file

```
git clone https://github.com/sjfarrar/sn-mid-server.git
cd sn-mid-server
docker build -t sn-mid-server .
```

## How to use this image

### just start a MID Server instance

This image includes EXPOSE 80 (the web services port)

```
docker run -d --name demonightlyeureka \
  -e 'SN_URL=demonightlyeureka' \
  -e 'SN_USER=admin' \
  -e 'SN_PASSWD=admin' \
  -e 'SN_MID_NAME=my_mid' \
  sn-mid-server
```

### create a config.xml file to use later
make sure to create the target directory first.
```
docker run --rm \
  -e 'SN_URL=demonightlyeureka' \
  -e 'SN_USER=admin' \
  -e 'SN_PASSWD=admin' \
  -e 'SN_MID_NAME=my_mid' \
  sn-mid-server mid:setup > /my_directory/demonightlyeureka/config.xml
```

### start with persistent storage (use the config.xml file you created earlier)

```
docker run -d --name demonightlyeureka \
  -v /my_directory/demonightlyeureka/logs:/opt/agent/logs \
  -v /my_directory/demonightlyeureka/config.xml:/opt/agent/config.xml \
  sn-mid-server
```

## Help
docker run sn-mid-server mid:help

    Available options:
     mid:start          - Starts the mid server (default)
     mid:setup          - Generate config.xml
     mid:help           - Displays the help
     [command]          - Execute the specified linux command eg. bash.
     
## TO DO
update expose to allow snmp listener + figure out networking
