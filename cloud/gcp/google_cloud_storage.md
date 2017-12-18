---
title: Google Cloud Storage
---

## Google Cloud Storage
google-cloud-sdkをインストールするとインストールされる`gsutil`を使う。

* Bucket名はGCS全体でunique
* Bucket名は、小文字、hyphen, underscore, 3-63文字、という制限がある
* google-cloud-sdkのgsutilがstorage用のCUI

* storage class
    * Multi Regional
        * 世界中で広く使われる場合
        * Web全般？
    * Regional
        * データの分析など一部の値域で使われる
    * Nealine
        * accessが殆どないもの
        * 月に1回くらい
    * Coldline
        * accessが殆どないもの
        * 年に1回くらい

## Upload

```
gsutil cp arcanine.png gs://hoge/sinmetal.png
```

## Change ACL

```
gsutil acl ch -u AllUsers:R gs://hoge/sinmetal.png
```

## Log
* [Access Logs & Storage Logs  |  Cloud Storage Documentation  |  Google Cloud Platform](https://cloud.google.com/storage/docs/access-logs)
* [Cloud Audit Logging with Cloud Storage  |  Cloud Storage Documentation  |  Google Cloud Platform](https://cloud.google.com/storage/docs/audit-logs)


Audito logと`access & storage log`の使い分け

* audit log
    * Cloud Audit Logging tracks access on a continuous basis.
    * Cloud Audit Logging produces logs that are easier to work with.
    * Cloud Audit Logging can monitor many of your Google Cloud Platform services, not just Cloud Storage.
* access & storage logを使いたい場合
    * You want to track access for public objects.
    * You use Access Control Lists (ACLs) to control access to your objects.
    * You want your logs to include latency information, or the request and response size of individual HTTP requests.
    * You want to track the amount of data stored in your buckets.


## CLI


```
gsutil mb gs://example-logs-bucket
```

* `mb`
    * make bucket

## Reference
* [Google Cloud Storageで適当にファイルを公開する方法 - Qiita](http://qiita.com/sinmetal/items/81395ce5fdaeb6e69310)

