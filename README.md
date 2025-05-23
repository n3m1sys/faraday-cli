# Faraday on the terminal
Use faraday directly from your favorite terminal

![Example](./docs/docs/images/general.gif)

faraday-cli is the official client that make automating your security workflows, easier.

## Install from pip

```
pip install faraday-cli
```

## Install from source
```shell script
git clone https://github.com/infobyte/faraday-cli.git
cd faraday-cli
pip install .
```

## Documentation

For more info you can check our [documentation][doc]


## Use it like a command

### Login

Configure auth for farday-cli

```shell script
$ faraday-cli auth
```
![Example](./docs/docs/images/auth.gif)


### Create a workspace
When you create a workspace by default is selected as active, unless you use the "-d" flag
```shell script
$ faraday-cli workspace create some_name
✔ Created workspace: some_name
```

### Select active workspace

```shell script
$ faraday-cli workspace select some_name
✔ Selected workspace: some_name
```

### List workspaces

```shell script
$ faraday-cli list_ws
NAME         HOSTS    SERVICES    VULNS  ACTIVE    PUBLIC    READONLY
---------  -------  ----------  -------  --------  --------  ----------
some_name       14          13       39  True      False     False
```

### List hosts of a workspace

```shell script
$ faraday-cli host list
  ID  IP           OS       HOSTNAMES          SERVICES  VULNS
----  -----------  -------  ---------------  ----------  -------
 574  127.0.0.1    unknown                            1  3
 566  127.0.0.10   unknown                            1  3
 569  127.0.0.11   unknown                            1  3
 568  127.0.0.12   unknown                            1  3
 570  127.0.0.13   unknown                            1  3
 576  127.0.0.2    unknown                            1  3
 565  127.0.0.3    unknown                            1  3
 572  127.0.0.4    unknown                            1  3
 573  127.0.0.5    unknown                            1  3
 567  127.0.0.6    unknown                            1  3
 571  127.0.0.7    unknown                            1  3
 564  127.0.0.8    unknown                            1  3
 575  127.0.0.9    unknown                            1  3
 590  58.76.184.4  unknown  www.googlec.com           0  -
```
### Update Hosts

```shell script
$ faraday-cli host update 12 -d '[{"ip":"127.0.0.1", "hostnames":["test.test.es"]}]'
Updated host:
{
    "services": 11,
    "owner": "faraday",
    "description": "",
    "ip": "127.0.0.1",
    "_rev": "",
    "workspace_name": "TEST",
    "hostnames": [
        "127.0.0.1",
        "feeds.cti.pers.eus"
    ],
    "name": "127.0.0.1",
    "vulns": 14,
    "metadata": {
        "update_user": null,
        "owner": "faraday",
        "update_controller_action": "",
        "update_action": 0,
        "command_id": null,
        "creator": "",
        "update_time": "2025-04-03T11:42:02.417316+00:00",
        "create_time": "2025-03-31T14:13:22.684505+00:00"
    },
    "_id": 12,
    "mac": "00:00:00:00:00:00",
    "type": "Host",
    "versions": [
        "OpenSSH 8.9p1 Ubuntu 3ubuntu0.11",
        "nginx 1.18.0",
        "",
        "Nagios NSCA",
        "",
        "Golang net/http server",
        "MinIO S3-compatible object store",
        "Elasticsearch REST API 8.17.3",
        "Elasticsearch binary API",
        "Golang net/http server"
    ],
    "id": 12,
    "service_summaries": [
        "(22/tcp) ssh (OpenSSH 8.9p1 Ubuntu 3ubuntu0.11)",
        "(80/tcp) http (nginx 1.18.0)",
        "(2377/tcp) grpc",
        "(7946/tcp) unknown",
        "(8000/tcp) nagios-nsca (Nagios NSCA)",
        "(8080/tcp) http-proxy",
        "(9000/tcp) http (Golang net/http server)",
        "(9001/tcp) http (MinIO S3-compatible object store)",
        "(9200/tcp) http (Elasticsearch REST API 8.17.3)",
        "(9300/tcp) elasticsearch (Elasticsearch binary API)",
        "(9443/tcp) https (Golang net/http server)"
    ],
    "owned": false,
    "default_gateway": "",
    "os": "Linux",
    "credentials": 0,
    "importance": 0
}
```
### Create Vuln

```shell script
$ faraday-cli vuln create -d '{"name": "process injection", "description": "inyeccion de procesos", "exploitation": "medium", "type": ["vulnerability_template"]}'
Vulnerability created with ID: 89
```

### Get host

```shell script
$ faraday-cli host get 574

$ faraday-cli host get 574
Host:
  ID  IP         OS       HOSTNAMES    OWNER    OWNED      VULNS
----  ---------  -------  -----------  -------  -------  -------
 574  127.0.0.1  unknown               faraday  False          3

Services:
  ID  NAME    DESCRIPTION    PROTOCOL      PORT  VERSION    STATUS      VULNS
----  ------  -------------  ----------  ------  ---------  --------  -------
2638  ssh                    tcp             22  unknown    open            2

Vulnerabilities:
   ID  NAME                                      SEVERITY    STATUS    CONFIRMED    TOOL
-----  ----------------------------------------  ----------  --------  -----------  -------
13509  SSH Weak Encryption Algorithms Supported  MED         opened    False        Openvas
13510  SSH Weak MAC Algorithms Supported         LOW         opened    False        Openvas
13511  TCP timestamps                            LOW         opened    False        Openvas
```

### Create hosts

```shell script
$ faraday-cli host create -d \''[{"ip": "stan.local", "description": "some server"}]'\'
```

Or pipe it
```shell script
$ echo '[{"ip": "1.1.1.5", "description": "some text"}]' | faraday-cli host create --stdin

```
**The escaping of the single quotes (\\') is only needed when using it as a command.
In the shell or using pipes it not necessary**


### Import vulnerabilities from tool report

```shell script
$ faraday-cli tool report "/path/to/report.xml"
```
![Example](./docs/docs/images/process_report.gif)

### Import vulnerabilities from command

```shell script
$ faraday-cli ping -c 1 www.google.com
```
![Example](./docs/docs/images/command.gif)

### List agents

```shell script
$ faraday-cli agent list
  id  name      active    status    executors
----  --------  --------  --------  -----------
   8  internal  True      online    nmap
```

### Run executor

```shell script
$ faraday-cli agent run -a 1 -e nmap -p \''{"target": "www.google.com"}'\'
Run executor: internal/nmap [{'successful': True}]
```



## Use it like a shell

Faraday-cli can be used as a shell and have all the same commands you have as a cli

![Example](./docs/docs/images/shell.gif)

## Use cases

### Continuous scan your assets with faraday

For example run nmap for all the hosts in faraday that listen on the 443 port and import the results back to faraday
```shell
$ faraday-cli host list --port 443 -ip | nmap -iL - -oX /tmp/nmap.xml  && faraday-cli process_report /tmp/nmap.xml
```

### Scan your subdomains

Use a tool like assetfinder to do a domains lookup, scan them with nmap and send de results to faraday

```shell
$ assetfinder -subs-only example.com| sort | uniq |awk 'BEGIN { ORS = ""; print " {\"target\":\""}
{ printf "%s%s", separator, $1, $2
separator = ","}END { print "\"}" }' | faraday-cli  agent run  -a 1 -e nmap --stdin
```


[doc]: https://docs.faraday-cli.faradaysec.com
