## date
dateコマンドについて。
BSD系とGNUでopstionが違う。

## Tips

### snippet
For GNU,

today

```
date +%Y%m%d
date +%Y-%m-%d
```

a day before today

```
date --date '1 day ago' +%Y%m%d
date --date '1 day ago' +%Y-%m-%d
```

For OSX

unixtime to date

```
date -r 1282368345
```

`-u`でUTC

```
date -u -r 1282368345
```

convert `20180101/` -> `2018/01/01/`

```
date -j -f "%Y%m%d/" 20180101/ +"%Y/%m/%d/"
```

## Tips

### Unixtime
Posix timeともいう。
UTCでの1970/1/1 00:00:00からの経過時間を表す。
UTC/JSTなどの設定に依存せずに時刻に変換できる。


## Reference
* [シェルスクリプトで一日前の日付を求める - すがブロ](http://sugamasao.hatenablog.com/entry/20060423/1145806183)
* [Unix time - Wikipedia](https://en.wikipedia.org/wiki/Unix_time)
