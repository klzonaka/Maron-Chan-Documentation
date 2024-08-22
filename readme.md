# Maron-Chan

Maron-Chanは、 世界最高レベルのもふもふを誇るもふもふベースで構成される

**もふもふに最適化されたもちもちでもふもふな言語です。**

# 構成

Maron-Chanは次の構成で成り立ちます。

* Maron-Chan プログラム
* 入力と、 出力、 エラー出力の**3つのバイトストリーム**

プログラムは、 `IMP <コマンド> <引数>`の形式をしています。

またMaron-Chanプログラムでは、 コードの終わりを表す記号として`！` `？` `。` `、` または 改行 があり、 複数の命令を行う場合にいずれかを使用する必要があります。

**IMP** (instruction manipulation parameter) は命令の種類を表しています。

|IMP||
|----|----|
|**もふ**|スタック操作
|**もちもふ**|演算|
|**もちもち**|ヒープアクセス|
|**もふもふ**|入出力|
|**もち**|フロー制御|

# 数値
引数として入力可能な <*Integer*\> は数値を表します。

最初に正の数か負の数であるかを指定し、 二進数で記入します。

**もふ**が正の数、 **もち**が負の数を表し、 後に続く値は**もふ**が1、 **もち**が0です。

```
# 例えば正の数で(1010)²は次のように表します。
もふもふもちもふもち

# 例えば負の数で(1010)²は次のように表します。
もちもふもちもふもち
```

# スタック操作
スタックに関連した操作を行います。 IMPは`もふ`です。

|コマンド <引数>|効果|
|----|----|
|**もふ** <*Integer*\>|スタックに整数値を積みます。 
|**もちもふ**|スタックの一番上の値を複製して積みます。|
|**もふもち**|スタックの上2つの値を交換します。|
|**もちもち**|スタックの一番上の値を捨てます。|
|**もふもふ** <*Integer*\>|整数 n を指定して、 スタックの上から n 番目にある値をコピーして、 スタックの一番上に積みます。|
|**もち** <*Integer*\>|整数 n を指定して、 スタックの一番上の値はそのまま残しつつ、 二番目以下を上から n 個削除します。|

# 演算
スタックの値を使用して計算を行います。IMPは`もちもふ`です。

演算に使用した値は捨てられます。

|コマンド <引数>|効果|
|----|----|
|**もふ**|スタックの上2つの値を加算し、 結果をスタックの一番上に積みます。| 
|**もちもふ**|スタックの上2つの値を減算し、 結果をスタックの一番上に積みます。|
|**もちもち**|スタックの上2つの値を乗算し、 結果をスタックの一番上に積みます。|
|**もふもふ**|スタックの上2つの値を除算し、 結果をスタックの一番上に積みます。 **結果は整数です。**|
|**もち**|スタックの上2つの値を除算した剰余 (除算した余り) をスタックの一番上に積みます。|

# ヒープアクセス
ヒープ領域と呼ばれるメモリに値を書き込み、 後から使用するのに役立ちます。 IMPは`もちもち`です。

|コマンド <引数>|効果|
|----|----|
|**もふ**|スタックからアドレスと値を取り出してヒープ領域に書き込みます。 **上から二番目の値がアドレス、 一番上の値が実際に書き込まれる値です。**|
|**もち**|アドレスから値を取り出してスタックに積みます。 **スタックとは違い、 読み取ったあとも値は残ります。**|

# 例外
Maron-Chanでは、 特定の条件でコードの実行を中断し、 エラー出力を行います。

## Syntax Error
構文の解析時に例外が発生した場合、 それ以降のコードの実行を中断します。
|例外|状況|
|----|----|
|Unknown IMP|IMPの解析に問題が発生。|
|Unknown command|コマンドの解析に問題が発生。|
|Unexpected arguments|コマンドは引数を期待していないが、 引数が渡された。|
|Invalid arguments|コマンドの引数に問題がある。|
|An argument received nil|コマンドは引数を期待しているが、 渡されなかった。|
|End point not found|コードの終了宣言が見つからなかった。|

## Stack Error
スタック時に問題がある場合、 それ以降コードの実行を中断します。
|例外|状況|
|----|----|
|Stack does not allow nil|スタックにnil値を積んだ。|
|Stack does not allow inf|スタックにinfinityを積んだ。|

## Expression Error
計算時に問題がある場合、 それ以降コードの実行を中断します。
|例外|状況|
|----|----|
|Cannot devide by zero|ゼロによる除算をした。|
|Expression includes nil|式にnil値が含まれている|
|Expression includes inf|式にinfinityが含まれている|

## Memory Warning
メモリアクセス時に問題がある場合、 ログに記録します。 **コードの実行は続行します。**
|例外|状況|
|----|----|
|Address not found, using nil|アドレスが見つからなかったため、 nilが読み込まれた。|
|Address will be deleted|アドレスに保存する値が見つからなかったため、 アドレスを削除した。|

## Link Error
実行前に解析され、 ラベルのリンク時に問題がある場合は、 コードを実行せずにエラーを記録します。
|例外|状況|
|----|----|
|Label is not resolved|ラベルのジャンプ先が不明。|
|Label declared but not used|ラベルは宣言したが使用されなかった。|
