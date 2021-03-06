---
title: Apache Spark
---

## Apach Spark

## spark standalone mode
* [Spark Standalone Mode - Spark 2.2.0 Documentation](https://spark.apache.org/docs/latest/spark-standalone.html)

```
./sbin/start-master.sh
```

```
http://localhost:8080
```

## Concept
* Node = Worker
    * nodeは複数のexecutorをもつ
* Executor
    * executorは複数のtaskをもつ
* task
* driver
* cluster manager

## Configuration
* [Configuration - Spark 2.3.0 Documentation](https://spark.apache.org/docs/latest/configuration.html)

### Spark UI
* `spark.eventLog.dir`
    * default: `file:///tmp/spark-events`
* `spark.ui.port`
    * default: `4040`

## Environment Variables
* [Configuration - Spark 2.3.0 Documentation](https://spark.apache.org/docs/latest/configuration.html#environment-variables)

幾つかの設定はenvironment variablesでできる。
`$SPARK_HOME/conf/spark-env.sh`から読み込まれるが、defaultでは作成されてない。
`$SPARK_HOME/conf/spark-env.sh.template`にtemplateがあるので、copyしてexecutableにする。


* JAVA_HOME
* PYSPARK_PYTHON
    * default: `python2.7` or `python`
    * `spark.pyspark.python`があればこちらが優先される
* PYSPARK_DRIVER_PYTHON
    * default: `PYSPARK_PYTHON`
    * python binary executable
* SPARKR_DRIVER_R
* SPARK_LOCAL_IP
* SPARK_PUBLIC_DNS


## API

### SparkConf
* [SparkContextメモ(Hishidama's Apache Spark SparkContext Memo)](http://www.ne.jp/asahi/hishidama/home/tech/scala/spark/SparkContext.html#h_SparkConf)

キー`spark.master`と値`hoge_master`などで保持されている。
使用できるconfigの一覧は以下。

* [Configuration - Spark 2.1.1 Documentation](https://spark.apache.org/docs/latest/configuration.html#available-properties)

* `spark.debug.maxToStringFields`
    * string型として扱うfieldの最大数
    * string型の出力はoverheadになるので、デフォルトで制限がある

* `spark.sql.shuffle.partitions`
    * データのshufleするときの分割数
    * 代数が少ない時は
    * [Spark SQL Programming Guide - Spark 1.2.0 Documentation](https://spark.apache.org/docs/1.2.0/sql-programming-guide.html)

## Tips

### nohup can't execute '--': No such file or directory
alpine linuxで以下のようなerrorがでる。

```
nohup: can't execute '--': No such file or directory
```

* [docker - CrashLoopBackOff in spark cluster in kubernetes: nohup: can't execute '--': No such file or directory - Stack Overflow](https://stackoverflow.com/questions/44661274/crashloopbackoff-in-spark-cluster-in-kubernetes-nohup-cant-execute-no-s)

この場合は

```
apk --update add coreutils
```

か、起動用のscriptを以下のように書き換える。

```
/usr/spark/bin/spark-submit --class org.apache.spark.deploy.master.Master $SPARK_MASTER_INSTANCE --port $SPARK_MASTER_PORT --webui-port $SPARK_WEBUI_PORT
```


### Reason: Container killed by YARN for exceeding memory limits.
* [apache spark - "Container killed by YARN for exceeding memory limits. 10.4 GB of 10.4 GB physical memory used" on an EMR cluster with 75GB of memory - Stack Overflow](https://stackoverflow.com/questions/40781354/container-killed-by-yarn-for-exceeding-memory-limits-10-4-gb-of-10-4-gb-physic)


### Skipped Stage
* [What does "Stage Skipped" mean in Apache Spark web UI? - Stack Overflow](https://stackoverflow.com/questions/34580662/what-does-stage-skipped-mean-in-apache-spark-web-ui)

Cacheのdataを利用可能で、stageを実行する必要がない場合にskipされる。
また、shufflingは多くのfileを生成するがこれらは、RDDが不要になるまで削除されるずに残る。
これらのfileが利用可能な場合もまた、stageがskipされる要因になる。

### File does not exist
* [hadoop - Spark Python submission error : File does not exist: pyspark.zip - Stack Overflow](https://stackoverflow.com/questions/34632617/spark-python-submission-error-file-does-not-exist-pyspark-zip)


### deploy mode
* [Deploy Mode · Mastering Apache Spark 2 (Spark 2.2+)](https://jaceklaskowski.gitbooks.io/mastering-apache-spark/spark-deploy-mode.html)

## Reference
* [Submitting Applications - Spark 1.6.0 Documentation](https://spark.apache.org/docs/1.6.0/submitting-applications.html)
* [Sparkアプリケーションの実行方法（spark-submit） - TASK NOTES](http://www.task-notes.com/entry/20160103/1451810637)
* [Deep Dive into Spark SQL's Catalyst Optimizer - The Databricks Blog](https://databricks.com/blog/2015/04/13/deep-dive-into-spark-sqls-catalyst-optimizer.html)
* [How-to: Tune Your Apache Spark Jobs (Part 2) – Cloudera Engineering Blog](https://blog.cloudera.com/blog/2015/03/how-to-tune-your-apache-spark-jobs-part-2/)* 
