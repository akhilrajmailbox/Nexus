# Nexus Server

## Prerequisites

* We are using `devops-network` docker network for all devops deployment.
* You need at least Docker Engine 17.06.0  and docker-compose 1.18 for this to work.

```bash
docker --version
docker-compose --version
```

## Create custom Docker network before deploying container (If the network is already created then ignore it)

*Note : Make sure that the subnet won't conflict with any other subnets*

```bash
docker network create --driver=bridge --subnet=172.31.0.0/16 devops-network
```

## Deploy Nexus as Docker container

```bash
docker-compose -f docker-compose.yml up -d
```


## Working with Nexus


* Login to Nexus server

```bash
docker exec -it nexus /bin/bash
```

* Restarting Nexus

```bash
docker-compose -f docker-compose.yml restart
```

* stopping/starting Nexus

```
docker-compose -f docker-compose.yml stop/start
```

* check the logs of Nexus server

```bash
docker logs -f nexus
```

## LDAP configuration

*Even Ldap configured, local user still can autheticate*

* Login as local admin in nexus ui 

* Goto [LDAP configuration page](https://my.example.com/nexus/#admin/security/ldap)

* Create the connection with following parameters

Configuration | Value
-------------|-------------
Name | `My DevOps LDAP`
LDAP server address | `ldap://ldap-server:389`
LDAP location to be added to the connection URL | `dc=my,dc=devops,dc=example,dc=com`
Authetication Method | `Simple Authentication`
Username or DN | `cn=Query Admin,ou=DevOps,ou=People,dc=my,dc=devops,dc=example,dc=com`
Password | `Above Username's Password`

* Verify connection

* Configure User and Groups in LDAP with following parameters

*User Configuration*

User Conf | Value
-------------|-------------
Configuration template | `Generic ldap server or empty`
User relative DN | `ou=People`
Object class | `posixAccount`
User filter | `(objectClass=posixAccount)`
User ID attribute | `uid`
Real name attribute | `cn`
Email attribute | `mail`
Password attribute | `empty`

*Group Configuration*

Group Conf | Value
-------------|-------------
Group type | `Static groups`
Map LDAP group as role | `true`
Group object class | `posixGroup`
Group ID attribute | `cn`
Group member attribute | `memberUid`
Group ID format | `${username}`


[ldap nexus configuration](https://help.sonatype.com/repomanager2/ldap-integration/user-and-group-mapping)