---
title: Wercker
---

## Wercker
表記について、werckerのorgazanizationとgithubのorgazanitionがあるので、これを区別する。
Githubのrepositoryとrepositoryを対象としたwerckerのci環境をapplicationと呼ぶ。

`wercker.yml`に設定を記述する。

* workflows
    * pipelinesの集まり
    * pipelinesの実行順序や実行条件などを記載した一連の処理
* pipelines
    * `wercker.yml`に記載する`build`や`deploy`, `test`、`dev`といったもの
    * pipelineは、stepsと呼ばれる処理の集まり
    * pipelineはweb uiで作成する
    * `report to SCM`にcheckすると、GitHunのPRなどに結果を反映できる。
* steps
    * stepの集まり
    * bashやコマンドの実行、インストールなど実際の処理
    * stpesは自分で記載することもできるが、communityで提供されているものも使うことができる
* box
    * docker imageのこと
    * boxはpipelineごとにも指定できる

アカウントの作成はHPから`logi->login with githubで`OK。
Appliationの作成は以下のようにする。

<div style="text-align: center">
    <img src="image/wercker_04_add_repository.png">
</div>

<div style="text-align: center">
    <img src="image/wercker_05_add_repository.png">
</div>

<div style="text-align: center">
    <img src="image/wercker_06_add_repository.png">
</div>


## Pythonのyamlの例
* [Getting started with Wercker & Python](http://devcenter.wercker.com/docs/quickstarts/building/python)

```yaml
# The container definition we want to use for developing our app
box: python:2.7-slim

dev:
  steps:
    # first we want to run pip-install to install all the dependencies
    - pip-install
    # then we want to run a wercker step that watches your files and reloads
    # when changes are detected.
    - internal/watch:
        code: python app.py
        reload: true
```

* `box: python:2.7-slim`
    * DockerHubのContainerを指定できる
    * 他のregistryも設定で登録可能
* `dev:`
    * development pipelineのコマンドなど
    * pipelineの中はstepで構成される
    * stepは自分で記述するか、werckerやwerckerのcommunityによって提供されるbash scriptなど
* `pip-install`
    * dev pipelineのstep

## Steps
Step registoryでcommunity から提供されているstepを利用できる。

* https://app.wercker.com/#explore/steps/search/

よく使うstepsは

* `script`
    * bash として実行
* `internal/watch`
    * web serverを使う場合に使う

### Internal Steps

* `internal/watch`
    * fileの変更で更新されるstep
    * よくあるのが、front-end developerがwebserverにファイルの変更を知らせる
    * 以下はnpmの例

```yaml
box: nodesource/trusty
dev:
  steps:
    - npm-install
    - internal/watch:
        code: node app.js
        reload: true
```

これを実行する場合は、

```
wercker dev --publish 5000
```

`5000`でserverを見ることができる。

OSXで使う場合は、一度に大量のファイルを監視すると制限に引っかかるため、以下のコマンドでしきい値を変更する。

```
sysctl -w kern.maxfiles=20480 (or whatever number you choose)
sysctl -w kern.maxfilesperproc=18000 (or whatever number you choose)
```

#### Internal/shell
shellのコマンドを実行したい場合は、`internal/shell`を使う。

* `cmd`
    * 起動するshell
* `code`
    * 実行するshellコマンド

```yaml
box: nodesource/trusty
dev:
  - npm-install
  - internal/shell:
      cmd: /bin/sh  #defaults to /bin/bash
      code: |
        # some code to automatically run in your shell session
        # before you start interacting
        cd /var/log
```

### Script Step
単純なshell commandを実行できる。
interna/shellとの使い分けは？

```yaml
build:
  steps:
    - script:
      name: indentify distribution
      code: cat /etc/lsb-release
    - script:
      name: starting xvfb
      code: |
        # Start xvfb which gives the context an virtual display
        # which is required for tests that require an GUI
        export DISPLAY=:99.0
        start-stop-daemon --start --quiet --pidfile /tmp/xvfb_99.pid --make-pidfile --background --exec /usr/bin/Xvfb -- :99 -screen 0 1024x768x24 -ac +extension GLX +render -noreset
        # Give xvfb time to start. 3 seconds is the default for all xvfb-run commands.
        sleep 3
```

### Creating Steps
Step registryに目的のstepがない場合は自分で定義することができる。

### Install Packages
containerに必用な依存ファイルをinstallする場合は、 `install-packages` stepを使う。
versionの指定も可能。

```yaml
- install-packages:
    packages: apache2=2.2.20-1ubuntu1
```


### After steps
step実行後に実行する場合は、`after-steps`というstepを使う。
piplelineの完了後のslackの通知などに使える。

```yaml
deploy:
    steps:
        - script:
            name: fabric deploy
            code: |
              fab deploy
    after-steps:
        - hipchat-notify:
            token: $HIPCHAT_TOKEN
            room_id: id
            from-name: name
```

## Pipelines
* [Pipelines](http://devcenter.wercker.com/docs/pipelines)

### Per-Pipeline Containers
pipelineごとにboxを指定できる。

```yaml
box: busybox
# this pipeline will use busybox
build-base:
  steps:
    - script:
      name: echo environment
      code: env
build:
  # override busybox
  box: google/golang
  steps:
    - script:
        name: testing build
        code: go version
```

## Workflows
Pipelinesの流れを記述する。
デフォルトでは、pipelienとしてbuildのみ登録されている。
よくある構成は

1. build
2. test
3. deploy
    * to staging
    * to production

である。
werckerでapplicationを作成したら、test pipelineの作成までは行った方が良い。
workflowに登録されているpipelineは、`wercker.yml`に必ず記載する必要がある。
将来的に使うpipelineとして登録しているが、現在特に処理するものがない場合は、以下のように空のstepsをいれる。

```yaml
build:
  steps:
test:
  steps:
    - script:
      name: execute pytest
      code: |
        pytest
```

<div style="text-align: center">
    <img src="image/wercker_01_workflow_pipeline.png">
</div>

<div style="text-align: center">
    <img src="image/wercker_02_manage_workflow.png">
</div>

<div style="text-align: center">
    <img src="image/wercker_03_manage_workflow.png">
</div>

## Wercker.yml
`dev`は特別なpipelineである。
`dev`はlocalのCLIで実行できるpipelineである。

## Environment variables
* [Available Environment Variables](http://devcenter.wercker.com/docs/environment-variables/available-env-vars)

いくつかのlevelで環境変数を指定できる。

* organization-level environment variable
* application-level environment variable
* pipeline-level environment variable

## Web Interface

### Repository Access
GitHubやBitBucketなどのrepositoryにアクセスする方法。
publicの場合は、public repositoryの節まで読み飛ばして良い。

### SSH keys
werckerはpublic keyをgithubのrepositoryに提供し、private keyでrepositoryのソースコードにアクセスする。
public keyを認可する方法はいくつかあるが、それぞれ良いことと悪いことがある。

* Deploy keys
    * deploy keyとしてpublic keyを登録する

## Organization
werckerのorganizationを作成できる。
teamで作業する場合に利用する。

### Creating Orgazantion
* organization name
* email

を記載して作成。

### Transfer Ownership
既存のwerckerのapplicationをwerckerのorgazanizationで管理するには、transfer ownershipでorgazanitzationに権限を移譲すれば良い。

### Adding an application
add applicationのときにownerを選ぶことができる。
ownerとしてorganizationを選ぶと`owner`はwerckerのapplicationに対するadminの権限を得る。

API Users

werckerのapplicationは、GithubやBitBucketのrepositoryからソースをcheckoutする。
その際に仕様するGithub/BitBucketのAPIの使用者を決めることができる。
defaultではorganizationの中で、werkcerのapplicationを作成したuserが選ばれる。
当然privateのrepositoryにアクセスする場合は、APIの使用者はそのrepositoryへのアクセス権限を持っている必要がある。

### People and teams
* [People and Teams](http://devcenter.wercker.com/docs/organizations/people-and-teams)


## Notification

### Slack
stepとしてpipelineの最後に以下を導入すればOK。
`$SLACK_URL`はwebhook urLを追加する。

```yaml
after-steps:
  - slack-notifier:
    url: $SLACK_URL
    channel: notifications
    username: werckerbot
    notify_on: "failed"
```

## CLI
* [The Wercker Command Line Interface (CLI)](http://devcenter.wercker.com/docs/cli)

### Install
Dockerが必要なので、適宜インストールしておく。

For OSX

```
brew tap wercker/wercker
brew install wercker-cli
```

## Tips

### Change repository name/ change url of repository
* [How can I update a repository URL?](http://devcenter.wercker.com/docs/faq/how-can-i-update-a-repo-url)

GitHubのrepositoryの名前の変更やrepositoryのURLの変更はUIからはできないので、supportにメール。


### Add organization's repository
* [Enabling OAuth App access restrictions for your organization - User Documentation](https://help.github.com/articles/enabling-oauth-app-access-restrictions-for-your-organization/)
* [Requesting organization approval for OAuth Apps - User Documentation](https://help.github.com/articles/requesting-organization-approval-for-oauth-apps/)
* [How do Webhooks work?](http://devcenter.wercker.com/docs/faq/how-do-webhooks-work)

後から参加した、githubのorganizationにAppsの承認を出す場合は、自分の`Authorized OAuth Apps`から該当のAppsをclickしてrequestボタンを押す。
承認されれば、自分のwerckerのアカウントから、githubのorganizationのrepositoryが見えるようになるので、werckerでApplicationの作成をできる。
Werckerの場合は、webhookが必要なので、repositoryへのadmin権限がある人間がwerckerでapplicationの作成した方が良い。

### Can I execute Docker commands inside my pipelines?
pipelineの中で`docker`コマンドは使えない。
stepsでいくつかのdocker commandのwrapperを使うことはできる。

## Samples
defaultで作られるyaml file

```yaml
# This references the default Python container from
# the Docker Hub with the 2.7 tag:
# https://registry.hub.docker.com/_/python/
# If you want to use a slim Python container with
# version 3.4.3 you would use: python:3.4-slim
# If you want Google's container you would reference google/python
# Read more about containers on our dev center
# http://devcenter.wercker.com/docs/containers/index.html
box: coorpacademy/docker-pyspark:latest
# You can also use services such as databases. Read more on our dev center:
# http://devcenter.wercker.com/docs/services/index.html
# services:
    # - postgres
    # http://devcenter.wercker.com/docs/services/postgresql.html

    # - mongo
    # http://devcenter.wercker.com/docs/services/mongodb.html

# This is the build pipeline. Pipelines are the core of wercker
# Read more about pipelines on our dev center
# http://devcenter.wercker.com/docs/pipelines/index.html
build:
  # The steps that will be executed on build
  # Steps make up the actions in your pipeline
  # Read more about steps on our dev center:
  # http://devcenter.wercker.com/docs/steps/index.html
  steps:
    # A step that sets up the python virtual environment
    - virtualenv:
        name: setup virtual environment
        install_wheel: false # Enable wheel to speed up builds (experimental)

    # # Use this virtualenv step for python 3.2
    # - virtualenv
    #     name: setup virtual environment
    #     python_location: /usr/bin/python3.2

    # A step that executes `pip install` command.
    - pip-install

    # # This pip-install clears the local wheel cache
    # - pip-install:
    #     clean_wheel_dir: true

    # A custom script step, name value is used in the UI
    # and the code value contains the command that get executed
    - script:
        name: echo python information
        code: |
          echo "python version $(python --version) running"
          echo "pip version $(pip --version) running"
```


## Reference
* [まだ CircleCI で消耗してるの？ - Qiita](http://qiita.com/KeithYokoma/items/b839ef3f5496a22f3e7a#_reference-3b29690796d83937e179)
* [Werckerの仕組み，独自のboxとstepのつくりかた | SOTA](http://deeeet.com/writing/2014/10/16/wercker/)
* [wercker の新機能 Wercker Workflows を試す - Qiita](http://qiita.com/dtan4/items/9bcf5dbfd3dbbd87472b)
