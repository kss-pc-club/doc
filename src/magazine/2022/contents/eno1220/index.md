# プログラミング言語 Zig の基本文法

<span class="author">eno1220</span>

## はじめに

こんにちは。6 期生の榎本です。パソコン部では、主に競技プログラミングをしたり、低レイヤーの勉強をしたりして遊んでいます。

さて、今回は Wikipedia を眺めていて発見した「Zig」<span class="footnote"> https://ziglang.org </span> と呼ばれるプログラミング言語を紹介したいと思います。

## Zig is 何？

Zig は、GitHub において、オープンソースで開発されているプログラミング言語です。2015 年ごろから開発が開始された比較的新しい言語で、現在は version 0.91 が最新版となっています。

Zig は、堅牢で最適かつ再利用可能なソフトウェアを維持するためのプログラミング言語ならびにツールチェーンとされています。

> Zig is a general-purpose programming language and toolchain for maintaining robust, optimal, and reusable software.

具体的には、メモリ不足などのエッジケースでも正確に動作し、プログラムが最適に動作/実行できるように記述され、同一コードが異なる多くの環境で動作するとされています。<span class="footnote">https://ziglang.org/documentation/0.9.1/#toc-Introduction</span>

![ロゴ](zig-logo-dark.svg){width=250}

## Zig を使うには？

Zig コンパイラは、公式サイト<span class="footnote">https://ziglang.org/download</span>で配布されているバイナリをダウンロードし、パスを通すか、パッケージマネージャを通じてインストールをすることができます。一部パッケージマネージャは過去のバージョンのみ対応しているものがあるので、注意が必要であるほか、最新版(master version)にはバグが含まれている場合があります。

```bash
$ export PATH=$PATH:~/Zig #ダウンロード後にパスを通す
$ brew install zig #もしくはhomebrewなどのパッケージマネージャを使用する
```

Vim や Vscode 用拡張機能や LSP も存在するので、GitHub やそれぞれのエディタのリポジトリなどを確認すると良いでしょう。

**これ以降の解説は 2022 年 5 月 17 日時点の公式ドキュメントや最新の安定版である Zig version0.9.1 をもとにしています。Zig はメジャーリリースに達しておらず、今後仕様変更の可能性があります。**

また、サンプルとなるコードは`sample.zig`として作成・実行しています。

## Zig コマンド

Zig コマンドのサブコマンドとして使用する形式になっています。

**主なコマンドの一覧**

- `zig run sample.zig` : sample.zig の実行ファイルを生成し、すぐに実行する
- `zig test sample.zig` : sample.zig の test コードを実行する
- `zig build`: build.zig の記述をもとにビルドを行う
- `zig fmt sample.zig` :smaple.zig にシンタックスフォーマットをかける

このほかにもさまざまなコマンドがあります。詳しくは、`zig --help`コマンドを参照のしてください。また、サブコマンドのオプションも多数存在し、こちらは`zig [sub command] -h`を参照してください。

## Hello,World!

さまざまなプログラミング入門書やリファレンスの初めには、`Hello,World!`と出力するプログラムが載っています。では、それに倣って Zig でも`Hello,World!`を出力してみましょう。

```zig
const std = @import("std");

pub fn main() void{
    std.debug.print("Hello,{s}!\n",.{"World"});
}
```

```bash:出力結果
$ zig run sample.zig
Hello,World!
```

まず、1 行目の`const std = @import("std")`では基本的な機能やアルゴリズム、データ構造などを提供する Zig Standard Library（標準ライブラリ）を組み込み関数である`@import`関数によって取り込んでいます。
続いて、`pub fn main() void`では、void 型の main 関数を定義しています。この関数がエントリーポイント（最初に実行される）になります。
main 関数のソースコードは、標準出力に`Hello,World!`と標準出力に出力をすることを表しています。
`std.debug.print` 関数の二つの引数はコンパイル時に評価され、第一引数の`{s}`は第二引数の値（ここでは`World`）に置き換えられます。`\n`はエスケープシーケンスであり、改行を示しています。なお、出力は`std.log`関数や`std.debug.print`関数によっても行うことができます。

## コメント

ソースコード中には機能の説明やメモなどの解説のためにコメントを書き込むことがあります。C 言語などでは複数行にわたってコメントを書くことが許されていますが、Zig は複数行に渡ったコメントは記述できません。

```zig
const std = @import("std");

pub fn main() void {
    // コメントは"//"から始まり、その行の最後までが範囲になる
    std.debug.print("Hello, world!\n", .{});
}
```

## 変数（整数・小数など）

Zig において、変数は `(var|const) 識別子 : 型 = 値`の形で表されます。`const`変数は不変の値であり、再代入が許されません。再代入により値を変えたい場合には、`var`変数を使用します。
`:型`ではその変数の型を示します。Zig ではその型のビット数を表します。なお、型推論が可能な場合、省略することが可能です。

- 符号付き整数型:`i8`,`i16`,`i32`,`i64`,`i128`
- 符号なし整数型:`u8`,`u16`,`u32`,`u64`,`u128`
- 浮動小数型:`f16`,`f32`,`f64`,`f80`,`f128`
- そのほか:`bool`,`noreturn`,`void`,`type`

など
上の整数型のほかに、任意のビット幅の整数を i もしくは u 識別子の後に数字をつけることで作成することができます。例えば、`u45`は符号なし 45bit 整数を表しています。

```zig
const std = @import("std");

pub fn main() void {
    var n: u8 = 50;
    n = n + 5; //nはvar変数なので代入可能

    const pi: u32 = 314159; //piはconst変数なので再代入不可かつ符号なし整数型なので負の数は表現できない

    const negative_eleven: i8 = -11;

    std.debug.print("{} {} {}\n", .{ n, pi, negative_eleven });
}
```

```bash:出力結果
$ zig run sample.zig
55 314159 -11
```

## 配列

Zig において、配列は`[要素数]要素型`の形で表されます。

```zig
const a = [3]i64 {100,200,300};
```

なお、要素数を推論できる場合には`_`を用いることができ、上の配列は以下のようにも表せます。

```zig
const b = [_]i64 {100,200,300};
```

配列の要素へのアクセスは `配列[添字]`で行うことができます。ただし、添字は C 言語や C++ などと同じく 0 から始まります。また、配列の要素数は`配列.len`で取得できます。
Zig の配列には面白い性質があり、`++`演算子で配列どうしの連結を、`**`演算子で配列を繰り返すことができます。

```zig
const std = @import("std");

pub fn main() void {
    var some_primes = [_]u8{ 1, 3, 2, 3, 5, 8, 13, 21 };

    some_primes[0] = 1;
    const first = some_primes[0];
    const fourth = some_primes[3];
    const length = some_primes.len;

    std.debug.print("First: {}, Fourth: {}, Length: {}\n", .{
        first, fourth, length,
    });

    const ko = [_]u8{ 2, 0 };
    const ga = [_]u8{ 1, 2 };
    const koga = ko ++ ga;

    const wa = [_]u8{ 202 } ** 3;

    std.debug.print("KOGA: ", .{});
    for (leet) |n| {
        std.debug.print("{}", .{n});
    }

    std.debug.print(", WA: ", .{});
    for (bit_pattern) |n| {
        std.debug.print("{}", .{n});
    }

    std.debug.print("\n", .{});
}
```

```bash:出力結果
$ zig run sample.zig
First: 1, Fourth: 3, Length: 8
KOGA: 2012, WA: 202202202
```

## 文字列

Zig では、文字列をバイト配列として扱います。C 言語と似ていますね！（実際は違うのですが...ここではそういうことにしておきます）

```zig
const a = "kss-pc-club";
const b = [_]u8{'k','s'**2,'-','p','c','-','c','l','u','b'};
```

文字列は変数 a のように`""`で囲むことで表すことができます。文字列をバイト配列として表しているので、a は b のようにも表すことができます。個々の文字はシングルクォート(`''`)、文字列はダブルクォート(`""`)で囲っていることに注意しましょう。これらは区別され、別のものです！

## 分岐(if)

Zig において分岐は以下のように書くことができます。C や C++と似たような構文ですね。

```zig
if(条件式){
    //条件式がtrueの時の処理
}else{
    //条件式がfalseの時の処理
}
```

Zig では条件式には真偽値のみを書くことができ、他の数値や他の型のデータを書くことができません。許されるのは、`true`もしくは`false`のみです。なので、以下のコードはコンパイルエラーになります。C や C++ではエラーとならず、実行することができますが、Zig ではできません。注意しましょう。

```zig
const std = @import("std");

pub fn main() void{
    const a = 1;
    if(a){
        //処理
    }
}
```

```bash:出力結果
$ zig run smaple.zig
./sample.zig:4:8 error: expected type 'bool', found 'comptime_int' #出力結果
```

また、以下のようにも書くことができます。Rust の構文に似ていますね。

```zig
const c:i8 = if(a) 100 else -100;
```

## 繰り返し(while/for)

Zig には for と while のループがあります。while は条件ループ、for はイテレータを用いるループに使うことが多いです。

### while

Zig の while はいくつかの書き方があります。なお、ループカウンタは while の外で宣言する必要があります。

```zig
const std = @import("std");

pub fn main() void{

    //ケース1:while(条件式){本体}

    var i:u32 = 1;
    while(i<10){
        std.debug.print("{} ",.{i});
        i += 3;
    }

    std.debug.print("{}\n",.{});

    //ケース2:while(条件式):(更新式){本体}
    //更新式は、ループの終わりに実行されます

    var j:u32 = 1;
    while(i<10):(i+=3){
        std.debug.print("{}",.{i});
    }

}

//ケース1とケース2は同じ結果を出力する

```

```bash:出力結果
$ zig run sample.zig
1 4 7
1 4 7
```

ケース 1 は C 言語でいう`while(条件式){}`、ケース 2 は C 言語でいう`for(;条件式;更新式)`のような形になっています。
また、ループの途中で`continue`式が登場した場合は、ループ内のそれ以下の処理を飛ばしループの初めに戻ります。また、ループの途中で`break`式が登場した場合は、ループを終了し、次の処理に移ります。

### for

for ループは配列に対して反復処理をするために使用されます。for は以下のような構文を持ちます。また、while ループと同様、`continue`や`break`を使用することができます。

```zig
const std = @import("std");

pub fn main() void{

    const array = [_]u8{'k','s','s'};

    for(array)|character|{
        std.debug.print("{u} ",.{character});
    }

    //forループでは配列のindexを持つこともできる

    for(array)|character,index|{
        std.debug.print("no{d} is {u}",.{@intCast(u32,index),character})
    }

}
```

```bash:出力結果
$ zig run sample.zig
k s s
no0 is k
no1 is s
no2 is s
```

サンプルコード中の`std.debug.print`関数の{}の中に`u`と書いていますが、これは第二引数が UTF-8 の文字であることを表しています。もしこれをしない場合、文字コードが出力されます。（この場合は筆者の環境においては、`107 115 115`と出力されました。）

## 関数

いままでも main 関数に代表されるようにさまざまな関数を使ってきましたが、使い方を詳しくみてみましょう。

```zig
const std = @import("std");

fn score(a:i32,b:i32) i32{
    return a * 10 + b * 30;
}

pub fn main() void{
    std.debug.print("{} {} {}\n",.{
        score(20,30),
        score(50,10),
        score(20,10),
    });
}
```

```bash:出力結果
$ zig run sample.zig
1100 800 500
```

上のサンプルコードでは`score`関数を作成しました。関数は、`fn 関数名(引数) 戻り値の型{}`というようになります。`score`関数では、a と b の引数をとり、戻り値は`i32`型になっています。また、引数は`識別子:型`というようにかくことができ、ここでは a、b ともに`i32`型の変数になっていることがわかります。
ところで、main 関数には`pub`というキーワードが書かれていますが、score 関数には書かれていません。`pub`属性は、他の Zig ファイルから`@import`された時に利用される関数・変数につけるものです。ただし、main 関数に限っては`pub`属性をつけるルールになっているので`pub`をつけています。

## おわりに

今回は、Zig の基本的な文法を紹介しました。本当はもっと書きたかったのですが、計画性のなさから締切を過ぎてしまい、これ以上記事を書く時間がないのでこれでおしまいにします。すみません。しかしこれだけでは Zig の機能がほとんど伝わっていないので、以下に紹介していない主な機能（特徴）を記します。

- C 言語と ABI 互換がある
- C 言語のプログラムを組み込むことができる
- 厳しい型制約、型推論がある
- コンパイラは C,C++をコンパイルできる
- webassembly 向けのビルドのサポートがある

などなど

私自身、勉強途中なためあまり詳しく解説ができず、またこのような記事を書くことが初めてのため拙い文章になってしまいました。お見苦しい部分が多かったと思いますが、最後まで読んでいただきありがとうございました。また、パソコン部を設立し、発展させてくださった諸先輩方、そしてこの部誌の発行ならびに部の各種運営を行なってくださっている部長に感謝申し上げ、結びといたします。

## 参考文献

- zig Language Reference / 英語 ([https://ziglang.org/documentation/master](https://ziglang.org/documentation/master))
- ziglern.org / 英語 ([https://ziglearn.org/](https://ziglearn.org/))
- ziglings / 英語 ([https://github.com/ratfactor/ziglings](https://github.com/ratfactor/ziglings))
- オープンソースプログラミング言語 Zig まとめ / 日本語　([https://qiita.com/bellbind/items/f2338fa1d82a2a79f290](https://qiita.com/bellbind/items/f2338fa1d82a2a79f290))

## 余談

実は日本語で Zig を解説している記事/サイトが 5 個くらいしかないのです。どなたか読者で日本語の記事を書いてくださる方はいませんか...
