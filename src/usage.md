# 使い方

ここでは、このドキュメントの編集方法を紹介します。
そもそもこのドキュメントは Rust で書かれた mdbook というものを使用していますので、そちらのドキュメント[^1]を参照していただくこともできます。
一部の Markdown 記法が使用できない場合や、数式の記法が独特なため、一度目を通しておくと良いでしょう。

## 解説の前に

こちらの解説では、Git や Github に関する説明は省略します。それらの使い方を確認したい場合は、茨城県プログラミングエキスパート事業で使用している[N 予備校](https://nnn.ed.nico)や、[GithubSkills](https://skills.github.com/)などをご覧ください。
また、必須ではありませんが、部活の Github Organisation に入ることもお勧めします。

## Github からリポジトリをクローンする

まずは、Github から、このドキュメントの元データが書かれたリポジトリをクローンしましょう。[リポジトリ](https://github.com/kss-pc-club/doc)にアクセスしてください。右上の Github のマークからもアクセスすることができます。

終わったら、ローカルのクローンしたリポジトリにアクセスしてみましょう。

## ディレクトリ構成

mdbook では、markdown ファイルでそれぞれの記事を作成します。`src`ディレクトリ以下に、ドキュメントの構成に必要なファイルを配置していて、markdown ファイルの他にも、css ファイルや画像ファイルなどを含めることができ、これらを使用して記事を書くことができます。

また、ディレクトリをきれいに管理するために、トピックごとにサブディレクトリを作成して管理をする方針をとっています。みなさんのご協力をお願いします。以下は、2022/7/29 時点の例です。

```
.
├── book.toml
└── src
    ├── SUMMARY.md
    ├── about.md
    ├── club-introduction.md
    ├── images
    │   └── homepage-screenshot.png
    ├── link.md
    ├── magazine
    │   ├── 2022
    │   │   ├── contents
    │   │   └── index.md
    │   └── index.md
    ├── self-introduction
    │   ├── 10th
    │   ├── 11th
    │   ├── 3rd
    │   ├── 4th
    │   ├── 5th
    │   ├── 6th
    │   │   └── eno1220
    │   │       └── index.md
    │   ├── 7th
    │   ├── 8th
    │   ├── 9th
    │   ├── README.md
    │   └── index.md
    └── usage.md

```

## 記事を書く

markdown ファイルを作成して、記事を書き始めましょう。なお、以下のような記法が可能です。主なものだけ抜粋して紹介します。詳細については、[こちら](https://rust-lang.github.io/mdBook/format/markdown.html)をご覧ください。

### 文字と段落

```text
This is example.

This is new line.
```

以下のようになります。

---

This is example.

This is new line.

---

### 見出し

見出しは、`#`マーカーを使用し、多く使うほど小さく表示されることを意味します。

```text
#### Big

some text.

###### Small
```

---

#### Big

##### Small

---

### リスト

```text
* apple
* google
* meta

1. C++
1. C
1. Python
```

---

- apple
- google
- meta

1. C++
1. C
1. Python

---

### リンク

他のファイルへは相対パスでリンクを書くことができます。

```text
たとえば [公式サイト](https://kss-pc.club).

詳しくは [こちら](link.md).

このドキュメントのURLは: <https://doc.kss-pc.club>.
```

---

たとえば [公式サイト](https://kss-pc.club).

詳しくは [こちら](link.md).

このドキュメントの URL は: <https://doc.kss-pc.club>.

---

### コードブロック

コードブロックは、以下のように` ``` `で囲んで記述します。主要な言語にはシンタックスハイライトが対応しています。

````text

```cpp

#include "iostream"

int main(){
    std::cout << "apple" << std::endl;
}

```

````

```cpp

#include "iostream"

int main(){
    std::cout << "apple" << std::endl;
}

```

mdbook では、Rust のコードを直接実行できます。コードブロックにマウスオーバーすると出てくる 3 つのアイコンの中で、真ん中のアイコンを押すと実行することができます。

```rust
println!("Hello KSS PC Club");
```

## SUMMARY.md に記事を登録する

mdbook では、ただ記事を書いただけでは表示させることができません。`src`以下の`SUMMARY.md`に記事のファイルへのリンクの記述を書く必要があります。逆に言えば、ファイルを記述しないことで、作成中の記事が web サイトからは閲覧できないようにすることも可能です。（そんなことをするタイミングがあるかと言えば...）

```markdown
[はじめに](./about.md)
[部活動紹介](./club-introduction.md)
[使い方](./usage.md)
[リンク](./link.md)

- [自己紹介](./self-introduction/index.md)
  - [eno1220](./self-introduction/6th/eno1220/index.md)
- [部誌](./magazine/index.md)
  - [2022](./magazine/2022/index.md)
    - [はじめに](./magazine/2022/contents/foreword.md)
    - [この部誌を支える技術](./magazine/2022/contents/about-vivliostyle/index.md)
```

リンクは以上のように記述します。階層化することが可能で、自動で章の番号を振り分けてくれます。ただし、一度リスト形式で記述すると、それ以降はリスト形式での記述が許されないので注意してください。

## プレビューする

プレビューをしたい場合は、mdbook をローカルにインストールする必要があります。インストールには cargo が必要ですので、インストールされていない方はまずインストールをしましょう。
mdbook のインストールは以下の通りです。

```shell
cargo install mdbook
```

mdbook はローカルサーバを立ててプレビューが可能です。以下のコマンドを実行すると、自動でブラウザが開きます。なお、ビルドされたファイル群は`book`ディレクトリ以下に展開されています。

```shell
mdbook serve --open
```

## Github に push する

プレビューができたら、Github に push しましょう。push ができたら、Github Actions によって自動でビルド&デプロイされ、2 分程度でサイトに反映されるはずです。

## おわりに

お疲れ様でした！みなさん、どんどん、記事を投稿しましょう！

[^1]: <https://rust-lang.github.io/mdBook/>
