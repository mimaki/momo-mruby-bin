# momo-mruby (mruby on GR-PEACH)

<table align="right"><tr><td><a href="README.md">English</a></td></tr></table>
<br/>

**momo-mruby**は、ルネサスエレクトロニクス製のマイコンボード [GR-PEACH](http://gadget.renesas.com/ja/product/peach.html) で動作する「[mruby](http://mruby.org/)」実行環境です。  
mrubyは、人気の開発言語「[Ruby](https://www.ruby-lang.org/)」を軽量化したプログラミング言語で、組込みシステムや様々なソフトウェアに組み込むことができる高機能なプログラミング言語です。  
momo-mrubyは、GR-PEACHマイコンボード上にmruby 1.3.0を動作せることで、以下の機能を提供します。

1. mrubyによる組込みアプリケーションの実行  
microSDに書き込んだmrubyアプリケーションを実行できます。  
mrubyコンパイラ(mrbc)でコンパイルしたmrubyバイナリ(mrbファイル)が実行できます。(mrubyスクリプトの実行もサポート予定)  
また、複数のアプリケーションを連続で実行することも可能です。

3. 対話型mruby(mirb)  
momo-mrubyでは、Rubyのirbに相当する対話型mruby(mirb)を動作させることができます。  
mirbでは対話形式でmrubyスクリプトを動作させることができるため、mrubyスクリプトの簡易実行やmrubyライブラリの動作確認等に利用できます。

3. mruby IoT フレームワーク Plato 対応  
momo-mrubyは、mruby IoT フレームワーク「[Plato](http://plato.click)」に対応しています。Platoで作成したIoTアプリケーションはmomo-mrubyで動作させることができます。


# momo-mrubyを使うための準備

## 準備するもの

- [GR-PEACH](http://gadget.renesas.com/ja/product/peach.html) (GR-PEACH-FULLを推奨)
- USBケーブル (A-MicroB)
- microSDカード
- Windows PC または Mac


## momo-mrubyソフトウェアの入手

以下のリンクからmomo-mrubyのソフトウェアをダウンロードします。

- [Windows用](https://github.com/mimaki/momo-mruby-bin/archive/1.0.0-win.zip)
- [Mac用](https://github.com/mimaki/momo-mruby-bin/archive/1.0.0-mac.zip)

ダウンロードしたmomo-mruby-1.0.0-xxx.zipを解凍すると以下のファイルが展開されます。

```
momo-mruby-1.0.0-xxx/
  +-- bin/
  |   +-- momo-mruby.bin (momo-mrubyファームウェア)
  |   +-- mrbc.exe (Macの場合はmrbc)
  +-- sample/
  |   +-- *.rb
  +-- README*.md (このファイル)
```


## GR-PEACHへのmomo-mrubyファームウェア書き込み

購入直後のGR-PEACHには、mruby実行環境(momo-mruby)は搭載されていません。  
GR-PEACHでmomo-mrubyを動作させるためには、GR-PEACHにmomo-mrubyファームウェアを書き込む必要があります。  
momo-mrubyのファームウェア書き込みは以下の手順で行って下さい。


1. GR-PEACH起動  
GR-PEACHをUSBケーブルでをPCに接続します。(2つあるUSBポートのうち、外側のポートに接続します)  
しばらくすると**MBED**という名前のドライブとして認識されます。

2. ファームウェアの書き込み  
momo-mrubyのファームウェア(momo-mruby.bin)をMBEDドライブにコピーします。  
しばらくすると（数秒以上かかります）MBEDドライブが再度マウントされ、momo-mrubyが起動します。


# mrubyアプリケーションの実行

momo-mruby起動時にmicroSDカードが装着されている場合は、ルートディレクトリにある**autorun.mrb**を実行します。  
mrubyアプリケーションのコンパイルには、mrubyコンパイラ(mrbc)を使用します。app.rbをコンパイルする場合には**bin**ディレクトリ内のmrbcコマンドを実行します。

```
mrbc -o <出力ファイル名> <mrubyスクリプトファイル名>
```

**例: sample/led.rbをコンパイル**

```
$ cd sample
$ ../bin/mrbc -o autorun.mrb led.rb
```

コンパイル結果(autorun.mrb)をmicroSDカードのルートディレクトリにコピーして、GR-PEACHのmicroSDスロットに装着します。  
microSD装着後、GR-PEACHをUSB接続して電源投入すると、momo-mrubyが起動し、mrubyアプリケーション(autorun.mrb)が実行されます。  
(led.rbを実行した場合はLEDがカラフルに点滅すれば動作していることが確認できます)


# momo-mrubyの対話モード(mirb)の起動

momo-mruby書き込み後のGR-PEACHをmicroSDを抜いた状態でPCにUSB接続します。  
ターミナルソフト(CoolTerm推奨)を起動して、GR-PEACHに接続します。

### Windowsの場合  
シリアルポートドライバをインストールする必要があります。  
[こちら](https://developer.mbed.org/handbook/Windows-serial-configuration)のページの**Download latest driver**のリンクからインストーラをダウンロードし、インストーラを実行して下さい。  
シリアルポートドライバのインストール後に、GR-PEACHをPCに接続すると、COMx に割り当てられます。

### Macの場合  
usbmodemXXXX に割り当てられます。

ターミナルソフトの設定は下記の通りです。

**Serial Port**
|設定項目|設定値|
|:--|:-:|
|Baudrate|9600|
|Data bits|8|
|Parity|none|
|Stop bits|1|

**Terminal**
|設定項目|設定値|
|:--|:-:|
|Terminal Mode|Line Mode|
|Enter Key Emulation|LF|
|Local Echo|ON|

**Connect**をクリックして、GR-PEACHに接続すると、mirbが開始されます。

**mribの実行例**
```
No disk, or could not put SD card in to SPI idle state
Fail to initialize card
mirb - Embeddable Interactive Ruby Shell

> MRUBY_VERSION
 => "1.3.0"
> p 'Hello, momo-mruby!'
"Hello, momo-mruby!"
 => "Hello, momo-mruby!"
> 
```

<font color="RoyalBlue">

### ポイント  
mrubyアプリケーションの終了後もmirbが実行されます。  

</font>
