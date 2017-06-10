---
title: gcloud
---

## gcloud

* GROUP
    * alpha
    * app
    * auth
    * beta
    * components
    * compute

## gcloud auth
oauth2のcredintialを扱う。

```
gcloud auth GROUP | COMMAND [GCLOUD_WIDE_FLAG …]
```


* Google Cloud SDKに対するauthorizationを行う

* GROUP
    * application-default

* Commands
    * active-service-account
    * list
    * login
    * revokE

```
gcloud config set account <service-account@gmail.com> 
```

で設定したアカウントに

```
gcloud auth login
```

でログインできる。
ログインすると、`~/.config/gcloud/credentials`にアクセスに必要なcredentialが書き込まれる。
gcloudはこのcredentialを使ってアクセスする。
中身は、アクセス用のjsonファイル。


## Reference
* [gcloud  |  Cloud SDK  |  Google Cloud Platform](https://cloud.google.com/sdk/gcloud/reference/)
