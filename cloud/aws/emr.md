## EMR
Elastic Map Reduce

* Hadoop
* Spark

両方使えるらしい。

## Steps
Clusterでの処理は、stepという形で追加する。
Cluster modeでは、S3に作業用のファイルなどをおく必要がある。
Client modeでは、ローカルのファイルを利用できる。
クラスタ作成時に、stepを指定おく方法と、作成後に`aws emr add-steps`などでstepを追加する方法がある。

* Type
    * typeごとに実行されるコマンドが決まっている
    * Spark, Hive, etc
* Name
    * stepの名前
* ActionOnFailure
    * failしたときにどうするか
    * CONTINUE
        * failしたときにINSTANCEを終了しない
* Args
    * 必要な引数
    * 配列として渡す

### Spark
* [Spark ステップの追加 - Amazon EMR](http://docs.aws.amazon.com/ja_jp/emr/latest/ReleaseGuide/emr-spark-submit-step.html)
* [Submitting Applications - Spark 2.1.1 Documentation](https://spark.apache.org/docs/latest/submitting-applications.html)

sparkの場合は、`spark-submit`コマンドが実行されるので、引数`Args`には、`spark-submit`に渡すものをわたす。


クラスタ作成時にstepを指定

```sh
aws emr create-cluster \
--name "Add Spark Step Cluster" \
--release-label emr-5.4.0 \
--applications Name=Spark \
--ec2-attributes KeyName=myKey\
--instance-type m3.xlarge
--instance-count 3 \
--steps Type=Spark,Name="Spark Program",ActionOnFailure=CONTINUE,Args=[--class,org.apache.spark.examples.SparkPi,/usr/lib/spark/lib/spark-examples.jar,10]
--use-default-roles
```

クラスタ作成時にstepを指定 with `command-runner.jar`

```sh
aws emr create-cluster \
--name "Add Spark Step Cluster" \
--release-label emr-5.4.0 \
--applications Name=Spark
--ec2-attributes KeyName=myKey
--instance-type m3.xlarge
--instance-count 3 \
--steps Type=CUSTOM_JAR,Name="Spark Program",Jar="command-runner.jar",ActionOnFailure=CONTINUE,Args=[spark-example,SparkPi,10]
--use-default-roles
```

作成済みのクラスタにstepを追加

```sh
aws emr add-steps
    --cluster-id j-2AXXXXXXGAPLF
    --steps Type=Spark,Name="Spark Program",ActionOnFailure=CONTINUE,Args=[--class,org.apache.spark.examples.SparkPi,/usr/lib/spark/lib/spark-examples.jar,10]
```

stepの情報を取得

```
aws emr describe-step
--cluster-id <value>
--step-id <value>
[--cli-input-json <value>]
[--generate-cli-skeleton <value>]
```


## Reference
* [AWS EMRを動かしてみよう。 - Qiita](http://qiita.com/uzresk/items/76ba0c9700e1d78fe5e3) 
* [（オプション）追加のソフトウェアをインストールするためのブートストラップアクションの作成 - Amazon EMR](http://docs.aws.amazon.com/ja_jp/emr/latest/DeveloperGuide/emr-plan-bootstrap.html)