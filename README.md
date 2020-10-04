# Universal Puyo Interface(UPI)

## 用語の定義

### ぷよ

- 赤、緑、青、黄、紫、お邪魔の六種類

### ツモ

- 操作対象となっている、上から落ちてくる、ぷよが2つくっついたものを表す 

### 配ツモ

- `ツモ`の出現パターンのこと
- 128回着手を行うと、出現パターンがループする

### ツモ番号

- 現在着手しようとしている配ツモが何番目なのかを表す値
- 0オリジンとする
- ゲーム開始後に、初めて着手するツモが、ツモ番号0である

#### 軸ぷよ、子ぷよ

- ツモの回転軸になっているぷよを`軸ぷよ`、そうでない方を`子ぷよ`とする
- `軸ぷよ`が下、`子ぷよ`が上の状態で落ちてくるものとする

### 盤面

- ツモを配置する空間のこと

### 盤面の表記方法

- 盤面上の座標を表記するときは、下記の表に従う

| |`1`|`2`|`3`|`4`|`5`|`6`|
|-|--|--|--|--|--|--|
|`m`|1m|2m|3m|4m|5m|6m|
|`l`|1l|2l|3l|4l|5l|6l|
|`k`|1k|2k|3k|4k|5k|6k|
|`j`|1j|2j|3j|4j|5j|6j|
|`i`|1i|2i|3i|4i|5i|6i|
|`h`|1h|2h|3h|4h|5h|6h|
|`g`|1g|2g|3g|4g|5g|6g|
|`f`|1f|2f|3f|4f|5f|6f|
|`e`|1e|2e|3e|4e|5e|6e|
|`d`|1d|2d|3d|4d|5d|6d|
|`c`|1c|2c|3c|4c|5c|6c|
|`b`|1b|2b|3b|4b|5b|6b|
|`a`|1a|2a|3a|4a|5a|6a|

### 指し手

- ツモを盤面に配置するための情報
- 以下2つの情報で表される
  - 軸ぷよの座標
  - 子ぷよの座標
- 軸ぷよの座標、子ぷよの座標の順で表記する
- (例) 1a1b なら 軸ぷよを`1a`、子ぷよを`1b`に配置することを表す

### 着手

- 指し手の通りに盤面にツモを配置すること

### 予告ぷよ

- 相手が連鎖を行った時、自分の盤面上部に、今いくつのお邪魔ぷよがたまっているかを表す特別なぷよが表示される
  - この上部に表示される特別なぷよのことを`予告ぷよ`と呼ぶ
    - 岩ぷよ、キノコぷよ、星ぷよ、王冠ぷよ等
- 自分の盤面に`予告ぷよ`が表示されている状態で着手を行った時、予告ぷよを発生させている連鎖が終了しているかどうかによって、以下2つの振る舞いを取る
  - 予告ぷよを発生させている連鎖が終了している場合
    - 着手を行うと、お邪魔ぷよが降る
  - そうでない場合
    - 着手を行っても、お邪魔ぷよは降らない
- 自分と相手の両方に`予告ぷよ`が表示されるということは発生しない
- つまり`予告ぷよ`には二種類あり、それぞれ以下の情報で表すことができる
  - 着手を行っても降らない予告ぷよ(未確定予告ぷよ)
    - お邪魔ぷよの数量
        - いくつお邪魔ぷよがたまっているか
        - どのプレイヤーに対する予告かは`符号`で表現する
            - 符号が+なら自分に対する予告、符号が-なら相手に対する予告  
    - 後何フレームで確定するか
  - 着手を行うと降る予告ぷよ(確定予告ぷよ)
    - お邪魔ぷよの数量

### pfen文字列

- 以下3つの組を表す文字列
  - 盤面の状態
  - ツモ番号
  - 予告ぷよの状態

- 以下のように書く
  - 自分の`盤面の状態` 自分の`ツモ番号` 相手の`盤面の状態` 相手の`ツモ番号` `予告ぷよ`

#### 盤面の状態の表記方法

- ぷよの種類はそれぞれアルファベット一文字で表す

|ぷよの色|文字列|
|-|-|
|赤|r|
|緑|g|
|青|b|
|黄|y|
|紫|p|
|お邪魔|o|

1. 盤面左下(`1a`)をスタートとする
2. 上方向にぷよを探し、ぷよがあればその色に対応するアルファベットを書く
3. その列についてぷよがない、もしくはm段目までぷよがあった場合は`/`を書く
4. もし右の列が存在しない場合は、終了。そうでなければ右の列のa段目に移動し、2. に戻る

- (例)
- 盤面にぷよが配置されていない場合のpfen文字列
  - //////
- 盤面の1aに赤、1bに黄が配置されている場合のpfen文字列
  - ry//////
- [kenny式19連鎖](https://www26.atwiki.jp/puyowords/pages/39.html)のpfen文字列
  - pppypppgpypy/yyygyggbgbbb/gggbybbybyyby/bbbpbyypyppyp/pppbyppgpggpg/yyybbggbbbgbg/

#### ツモ番号の表記方法

- 現在の`ツモ番号`をそのまま文字列として書く

#### 予告ぷよの表記方法

- 以下のことを書く
    - 確定予告ぷよ  
        - `お邪魔ぷよの数量`
    - 未確定予告ぷよ
        - `お邪魔ぷよの数量`
        - `後何フレームで確定するか`
- (例) 1pに確定予告ぷよが34こ、未確定予告ぷよが239個あり、後93フレームで確定する場合
  - 34 239 93

## 通信プロトコルについて

- 参考. USIプロトコル
- http://shogidokoro.starfree.jp/usi.html

### GUI → エンジン

|コマンド|意味|
|-|-|
|upi|upiエンジンであることを認識するためのコマンド。エンジンはこれを受け取ったら即座に`id`コマンドを返し、最後に`upiok`を返すこと。|
|rule <br>falltime &lt;f&gt;<br>chaintime &lt;f&gt; <br>settime &lt;f&gt; <br>nexttime &lt;f&gt; <br>autodroptime &lt;f&gt;|対戦ルールを決めるためのコマンド。<br>falltime: 下ボタンを押しっぱなしにしたとき、何フレームで1マス落下するか<br>chaintime: 1連鎖につき何フレーム硬直するか<br>settime: ツモ設置時に何フレーム硬直するか<br>nexttime: ツモ設置硬直後、または連鎖終了後、またはお邪魔ぷよが振り終わった後、ネクストが操作可能になるまでに何フレーム硬直するか<br>autodroptime: 何も操作しなかった問、何フレームで1マス落下するか|
|tumo &lt;c&gt;&lt;c&gt; &lt;c&gt;&lt;c&gt; ...| ゲームで使用されるツモを表す。&lt;c&gt;はr\|g\|b\|p\|yのいずれか。&lt;c&gt;を2つ合わせて一組とし、スペース区切りで128組送る。isreadyコマンドよりも前に送る。|
|position &lt;pfen&gt;|盤面の状態をpfen文字列で送る。詳細はpfen文字列を参照。|
|isready|ゲーム開始前に送る。エンジンは対局準備ができたら`readyok`を返すこと。|
|go [&lt;millisecond&gt;]|思考開始の合図。エンジンはこのコマンドを受信したら、`bestmove`コマンドを返すこと。millisecondが指定されていた場合は、指定したミリ秒以内に`bestmove`コマンドを返さなかった場合、もう一度goコマンドが送られる。その間のツモ操作は行われない。|
|illegalmove &lt;move&gt;|`bestmove`により送られてきた指し手が非合法手だった場合に送り返されるコマンド。この後すぐにgoコマンドが送られるので、もう一度`bestmove`を返すこと。|
|quit|アプリケーション終了を命令するコマンド。|
|gameover &lt;win\|lose&gt;|ゲーム終了を表すコマンド。|

### エンジン → GUI

|コマンド|意味|
|-|-|
|id name &lt;program name&gt;<br>id author &lt;program name&gt;|upiコマンドを受信したときに最初に送り返すコマンド。id nameでプログラム名を返し、id authorで作者名を返す。|
|upiok|upiコマンドを受信したときに最後に送り返すコマンド。これを送らないとupiエンジンと認識されない。|
|readyok|isreadyコマンドを受信したときに送り返すコマンド。重い処理を行う場合はこのコマンドを送り返す前に行う。|
|bestmove &lt;move&gt;|goコマンドを受信したときに送り返すコマンド。エンジンの指し手を返す。もし非合法手だった場合、`illegalmove`コマンドを返す。|

### 通信の例

- わかりやすくするために、コマンドの前に以下をつけて表記する
  - GUI → エンジン のコマンドには">"
  - エンジン → GUI のコマンドには"<"

#### ゲーム開始前

```
// エンジン起動
>upi

// upiコマンドに対する返答(例)
<id name defaultEngine1.0
<id author Ryuzo Tukamoto
<upiok

// 配ツモを送る
>tumo ry rb gy gb rg yy rb gg yg rg yb gr rr yy by gr bb rb yb bg rb gy gy gy yy yr yg yy rg rb bg br rr by gy rb bg gg yr yy rg rb by by gb yy bg bg rg yg rg yb yg yr yr rr br br gr gb rr yr rb gg gg bg gy bb yg gg gb yr yg yb bg br gy br rb gy rr rr yb gy by gb yy gr bb yy gb gr rr by yb gb yr yg rr gg yy yy gy by gr br rg gy yg gr yb by bg yr yr bb rr br yr gr gr yg bb yr rr gb yg gy

// ルールを送る
> rule falltime 2 chaintime 60 settime 15 nexttime 7 autodroptime 50

// 対局開始前の準備
>isready
<readyok
```

#### 指し手のやりとり

```
// 自分の盤面、自分のツモ番号、相手の盤面、相手のツモ番号、確定予告ぷよ、未確定予告ぷよ、確定するまでのフレーム数
>position ////// 0 ////// 0 0 0 0
>go 300

<bestmove 1a1b

>position ry////// 1 ////// 0 0 0 0
>go 300

<bestmove 1c1d

>position ryrb////// 2 ////// 0 0 0 0
>go 300

<bestmove 1e1f
...

// 試合終了
>gameover win
```
