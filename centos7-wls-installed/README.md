# centos7-wls-installed

CentOS 7 に WebLogic Server 12.2.1.4 をインストールしたコンテナ。WebLogic が稼働するわけではない。構築検証用に。

## Prerequisites

* \>512 MB swap on host

## Dependency

* files/fmw_12.2.1.4.0_wls_lite_generic.jar
  * Oracle WebLogic Server 12.2.1.4 Generic Installer
* files/jdk-8u291-linux-x64.rpm
  * Oracle Java SE Development Kit 8u291 (Linux x64 RPM Package)

## Build

```
$ docker build -t local/centos7-wls-installed
```

## Run

systemd を有効にするために `-d --privileged` オプション付きでコンテナを作成したあとでコンテナに入る必要がある。

```
$ docker run -d --privileged local/centos7-wls-installed
$ docker exec -it <CONTAINER_ID> /bin/bash
```
