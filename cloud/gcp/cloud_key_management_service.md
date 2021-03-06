---
title: Cloud Key Management Service
---

## Cloud Key Management Service

* localtion
    * [Cloud KMS locations  |  Cloud KMS Documentation  |  Google Cloud](https://cloud.google.com/kms/docs/locations)
* keyring
    * localtionをきめる
    * keyringには複数のkeyを作成できる
* key
    * keyはrotationされる

## CLI

```
gclodu kms keys create
```

## Usage
create keyring

```
gcloud kms keyrings create KEYRING \
  --location=global
```

create key without rotation

```
gcloud kms keys create KEY_NAME \
    --location LOCATION \
    --keyring KEYRING_NAME \
    --purpose encryption
```

set key rotation period with rotation

```
gcloud kms keys create KEY_NAME \
    --location LOCATION \
    --keyring KEYRING_NAME \
    --purpose encryption \
    --rotation-period ROTATION_PERIOD \
    --next-rotation-time NEXT_ROTATION_TIME
```

`secrets.json`からencryptした`secrets.json.enc`を生成する。
repositoryにfileごと含めることができる。

```
gcloud kms encrypt \
  --plaintext-file=secrets.json \
  --ciphertext-file=secrets.json.enc \
  --location=global \
  --keyring=[KEYRING-NAME] \
  --key=[KEY-NAME]
```

環境変数をencryptして出力

```
echo -n $MY_SECRET | gcloud kms encrypt \
  --plaintext-file=- \  # - reads from stdin
  --ciphertext-file=- \  # - writes to stdout
  --location=global \
  --keyring=[KEYRING-NAME] \
  --key=[KEY-NAME] | base64
```

decrypt

```
gcloud kms decrypt \
    --location=global \
    --keyring=my-key-ring \
    --key=my-key \
    --ciphertext-file=YOUR_FILEPATH_AND_FILENAME_TO_DECRYPT \
    --plaintext-file=YOUR_FILEPATH_AND_FILENAME_TO_DECRYPT.dec
```

## Permissions and Roles
* Roleはkeyring, keyごとに、accountを指定して付与できる
    * keyring Aについて、account Bにrole Cを付与
    * key Aについて、account Bにrole Cを付与

* `roles/cloudkms.cryptoKeyDecrypter`
    * decryptできる
* `roles/cloudkms.cryptoKeyEncrypter`
    * encryptできる

## Rotations
* rotateしてもold keyはdisableにはならない
* old keyがenabledの間はold keyでenryptされたものは、decryptできる
* old keyをdisableすれば、old keyでencryptされたものはdecryptできない
    * old keyでencryptしたfileが流出した場合
    * keyをrotateしてnew keyを作る
    * old keyをdisableにする
    * old keyでencryptしていたものnew keyでencryptする

## Destroying and restoring key versions
* [Destroying and restoring key versions  |  Cloud KMS Documentation  |  Google Cloud](https://cloud.google.com/kms/docs/destroy-restore)

* key version
    * key versionはdelete, disableがありそれぞれ権限がある
    * deleteは24hour後に削除される
        * 取り消しは可能
        * disableにschedulingされる
    * 削除されたversionで暗号化されたものは復元不能
    * 削除されたversionは再利用されない
* key
    * [Object hierarchy  |  Cloud KMS Documentation  |  Google Cloud](https://cloud.google.com/kms/docs/object-hierarchy#lifetime)
    * 名前の再利用をさけるため、削除できない
* keyring
    * [Object hierarchy  |  Cloud KMS Documentation  |  Google Cloud](https://cloud.google.com/kms/docs/object-hierarchy#lifetime)
    * 名前の再利用をさけるため、削除できない


key version削除できる権限は限られている

* `role/cloudkms.admin` or `role/owner`
    * key versionを削除できる

## Pricing
* [Cloud KMS Pricing  |  Cloud KMS Documentation  |  Google Cloud](https://cloud.google.com/kms/pricing)

* active key当たり 0.06USD/month
    * Enabled
    * Disabled
    * Scheduled for destruction
* Encrypt or Decrypt operationは0.03USD per 10,000 operations



## Reference
* [Cloud Key Management Service Documentation  |  Cloud KMS Documentation  |  Google Cloud](https://cloud.google.com/kms/docs/)
* [Google Cloud Platform Blog: Managing encryption keys in the cloud: introducing Google Cloud Key Management Service](https://cloudplatform.googleblog.com/2017/01/managing-encryption-keys-in-the-cloud-introducing-Google-Cloud-Key-Management-Service.html)
