---
title: Alluxio 分散式虛擬儲存系統
date: 2016-5-06 17:08:54
layout: page
categories:
- Spark
tags:
- Spark
- Java
- Storage
---
`Alluxio` 是分散式虛擬儲存系統，早期名稱為 Tachyon ，而現在已正式改名 Alluxio，並發佈 1.0 版本

Aluxion 是一個記憶體虛擬分散式儲存系統，具有高效能、高容錯以及高可靠性的特色，它能夠統一資料的存取去串接機算框架和儲存系統的橋梁，像是同時可相容於 Hadoop MapReduce 和 Apache Spark 以及 Apache Flink 的計算框架和 Alibaba OSS、Amazon S3、OpenStack Swift,、GlusterFS 及 Ceph 的儲存系統

![](/images/spark/alluxio_architecture.jpg)

<!--more-->

## Install Java7
首先安裝相關套件：
```sh
$ sudo apt-get purge openjdk*
$ sudo apt-get -y autoremove
$ sudo add-apt-repository -y ppa:webupd8team/java
$ sudo apt-get update
$ echo debconf shared/accepted-oracle-license-v1-1 select true | sudo debconf-set-selections
$ echo debconf shared/accepted-oracle-license-v1-1 seen true | sudo debconf-set-selections
$ sudo apt-get -y install oracle-java7-installer
```

## Download Alluxio 1.0.0
接著下載 Alluxio：
```sh
$ wget http://alluxio.org/downloads/files/1.0.0/alluxio-1.0.0-bin.tar.gz
$ tar xvfz alluxio-1.0.0-bin.tar.gz
$ cd alluxio-1.0.0
```

複製一個`conf/alluxio-env.sh`檔案
```sh
$ cp conf/alluxio-env.sh.template conf/alluxio-env.sh
```

`conf/alluxio-env.sh `中加入`ALLUXIO_UNDERFS_ADDRESS`參數

```sh
export ALLUXIO_UNDERFS_ADDRESS=/tmp
```

確認 `ssh localhost` 可成功
```sh
$ssh-copy-id localhsot
```

格式化 Alluxio FileSystem 並開啟它
```sh
$ ./bin/alluxio format
$ ./bin/alluxio-start.sh local
```

驗證 Alluxio 可於瀏覽器輸入`http://localhost:19999`，也可執行簡單的程式，如下:

```sh
$ ./bin/alluxio runTest Basic CACHE THROUGH
```

執行後，如下:
```
2015-11-20 08:32:22,271 INFO   (ClientBase.java:connect) - Alluxio client (version 1.0.0) is trying to connect with FileSystemMaster master @ localhost/127.0.0.1:19998
2015-11-20 08:32:22,294 INFO   (ClientBase.java:connect) - Client registered with FileSystemMaster master @ localhost/127.0.0.1:19998
2015-11-20 08:32:22,387 INFO   (BasicOperations.java:createFile) - createFile with fileId 33554431 took 127 ms.
2015-11-20 08:32:22,552 INFO   (ClientBase.java:connect) - Alluxio client (version 1.0.0) is trying to connect with BlockMaster master @ localhost/127.0.0.1:19998
2015-11-20 08:32:22,553 INFO   (ClientBase.java:connect) - Client registered with BlockMaster master @ localhost/127.0.0.1:19998
2015-11-20 08:32:22,604 INFO   (WorkerClient.java:connect) - Connecting local worker @ /192.168.2.15:29998
2015-11-20 08:32:22,698 INFO   (BasicOperations.java:writeFile) - writeFile to file /default_tests_files/BasicFile_CACHE_THROUGH took 311 ms.
2015-11-20 08:32:22,759 INFO   (FileUtils.java:createStorageDirPath) - Folder /Volumes/ramdisk/alluxioworker/7226211928567857329 was created!
2015-11-20 08:32:22,809 INFO   (LocalBlockOutStream.java:<init>) - LocalBlockOutStream created new file block, block path: /Volumes/ramdisk/alluxioworker/7226211928567857329/16777216
2015-11-20 08:32:22,886 INFO   (BasicOperations.java:readFile) - readFile file /default_tests_files/BasicFile_CACHE_THROUGH took 187 ms.
Passed the test!
```

執行更複雜的測試:
```sh
$ ./bin/alluxio runTests
```

停止服務：
```sh
$ ./bin/alluxio-stop.sh all
```
