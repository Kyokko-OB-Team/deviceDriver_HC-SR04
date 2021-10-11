# 自作DeviceDriver

testアプリのソースコードも参考にしてください。

## 目次

- [デバイスドライバを使うには](#デバイスドライバを使うには)
- [HC-SR04](#HC-SR04)


## デバイスドライバを使うには

デバイスドライバはカーネルモジュールとも呼ばれます。<br>
カーネルモジュールは、カーネルと同じソースからビルドする必要があるため、<br>
カーネルにあったデバイスドライバを使用する必要があります。<br>
カーネルバージョンを上げたり、カーネルソースを変更した場合は<br>
カーネルモジュールもビルドし直す必要がありますのでご注意ください。<br>
<br>
このカーネルモジュールが使用可能なカーネルは、 [こちら](https://github.com/Kyokko-OB-Team/deviceDriver/blob/master/kernel.img) にございます。<br>
/bootディレクトリ配下のkernel.imgと差し替えることで、<br>
本リポジトリのカーネルモジュールをロードすることができるようになります。<br>
差し替えは、以下のように元のカーネルイメージをバックアップしてから行うのがおすすめです。<br>

```
$ sudo cp /boot/kernel.img /boot/kernel-bk.img
$ sudo cp ~/deviceDriver/kernel.img /boot/.
```

<br>
カーネルモジュールは以下の方法で、ロード(カーネルに組み込む)することが出来ます。<br>

```
$ sudo insmod hc_sr04.ko
```

<br>
カーネルに組み込んだあとは、/devディレクトリ配下にデバイスファイルが作成されます。<br>
デバイスドライバを使用する場合は、こちらのデバイスファイルに対して操作を行います。<br>
<br>
カーネルからアンロードしたい(取り外したい)場合は以下のようにします。<br>

```
$ sudo rmmod hc_sr04
```


## HC-SR04

超音波距離センサで距離を取得するためのデバイスドライバです。<br>
コマンド `GPIO_HCSR04_EXEC_MEASURE_DISTANCE` で距離の計測要求を行い、<br>
`GPIO_HCSR04_GET_DISTANCE` で計測結果を取得します。<br>
<br>
センサの計測時間がありますので、<br>
`GPIO_HCSR04_EXEC_MEASURE_DISTANCE` から `GPIO_HCSR04_GET_DISTANCE` の実行までは<br>
1秒程度の時間を開けてください。<br>
<br>
<br>
| コマンド | データ | 方向 | 説明 |
|:---------|:------:|:----:|:-----|
| `GPIO_HCSR04_EXEC_MEASURE_DISTANCE` | - | out | 距離の測定要求 |
| `GPIO_HCSR04_GET_DISTANCE` | 距離データ(mm) | in | 測定した距離の取得 |

