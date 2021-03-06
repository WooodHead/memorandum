---
title: Kinesis Firehose
---

## Kinesis Firehose
Amazon Kinesis Analytics、Amazon S3、Amazon Redshift、および Amazon Elasticsearch Serviceにstreaming dataをloadできる。

* shardによる性能調整が不要
    * 投入データ料におうじた従量課金
* 1秒あたり2500回のPUTが可能、サポート経由で上限緩和が可能

## Firehose/streams
* Kinesis Streams
    * EC2、EMR、Lamnda等、複数のアプリケーションから参照する事が可能です。
    *  ャードの増減により性能調整が可能です。
* kinesis firehose
    * 登録可能なデータの仕様はKinesis Streamsと共通です。
    * 登録されたデータは、S3、Redshiftに自動的にエクスポートされます。
* Kinesis analytics
    * Amazon Kinesisに登録されたストリームデータのタイムウィンドウ処理、直近の一定時間を対象とした集計などがSQLにより可能となります。

## Price
* 最初の500TB/monthまで
    * 1GBあたり\$0.029
* 次の1.5PB/monthまで
    * 1GBあたり\$0.025
* 次の3PB/monthまで
    * 1GBあたり\$0.020
* 5PB/month以上
    * 問い合わせが必要


## Reference
* [AWS再入門 Amazon Kinesis編 ｜ Developers.IO](http://dev.classmethod.jp/cloud/aws/cm-advent-calendar-2015-aws-re-entering-kinesis/)

