---
title: "How to build your blog"
date: 2022-11-30T02:59:10+09:00
---

こんにちは!42tokyo Advent Calendar 2022の14日目を担当する, knittaです。よろしくお願いします。

### **はじめに**
---

突然ですが，皆さんこういうことを思ったことがないでしょうか。


> **「あー，なんか有無を言わさずみんなに褒められて，気分良くなりたいなぁ...」**


こんなときは，みんなと同じことをやっていてはいけない，ということで私は**42Tokyo** というところの入学試験(Piscine)を受けて見事合格しました。それから僕は友達を集めて，「ねぇみんな聞いてー！僕,42Tokyoに合格したよ〜〜〜」と言いました。すると，みんなこう言いました。


> **「なにそれ〜」**

この様な結果から次の2点の事実が観察できます。一つは
1. 「42Tokyo の知名度は低く，その価値は社会から認識されていない」
という点。

もう一つは，

2. 「みんなと違うことをやったからと言って，それだけで誉められる訳ではない」
という点です。

### **今回のテーマ**
---
以上の観察から私はこう思いました。

> 「42Tokyo の価値をみんなに知ってもらって，かつ，みんなに褒められた〜い」

そのためには， 42Tokyo がどの様な場所で，私がどの様な人間であるかを知ってもらうための情報を発信する方法が必要になると思います。また，なんかかっこいいやり方でそれを実現するのがよいのではないでしょうか。カッコいいほうが自慢できるし，誉められそうなので😄

### **静的サイトジェネレータ + Github Pages = cool**
---
検索エンジンを駆使して情報を集めていった結果，
- 静的サイトジェネレータ


- Github Pages


というものを使えば，カッコいいサイトを作って，Github で公開することができそうだぞ，ということがわかりました。これを使って42Tokyoのことや，私が今やっていることについて，coolに情報を発信することができるのではないかな？

では，早速やってみましょう！

...

とその前に一応これらの用語について用語を整理しておきます。

[静的サイトジェネレータ](https://en.wikipedia.org/wiki/Static_site_generator) ... Markdown などの記法で書かれたテキストファイルから，静的なWebページを生成するソフトウェアである

[Github Pages](https://docs.github.com/ja/pages/getting-started-with-github-pages/about-github-pages) ... 静的サイトのホスティングサービスであり，これを使って静的サイトジェネレータを使って作った Webサイトを運用することができる。

### **実際にやってみる**
---
では，実際に静的サイトジェネレータを使って，Webサイトを作ってから，Github Pages にデプロイしてみましょう。今回私が使う静的サイトジェネレータは，[Hugo](https://gohugo.io/)というものです。

私の環境(M1 Mac)では，Homebrew というパッケージマネージャを使って，Hugo をシステムに導入することができました。

```
$> brew install hugo
```

これだけで，Hugo を使って静的サイトを作ることができます。まずは，サイトの雛形を作りましょう。以下のようなコマンドを打って実行するだけでいい。

```
$> hugo new site <サイト名>
```

<サイト名>で指定してディレクトリが新しくできるので，このディレクトリに移動して，`tree` コマンドを打ってみると以下の様な構成になっていることがわかリます。

```
$> tree
.
├── archetypes
│   └── default.md
├── assets
├── config.toml
├── content
├── data
├── layouts
├── public
├── static
└── themes
```

このディレクトリは，Github Pages を使ってホスティングするために，Github で管理するため，以下のコマンドで git リポジトリにしておきます。

```
$> git init
```

Hugo では`content`ディレクトリに，Markdown ファイルを追加することで，このファイルをサイトで公開することができます。`hugo new` コマンドで，`content`ディレクトリ以下に，記事ファイルを追加することができるので，テスト用のファイルを追加しておきましょう。

```
$> hugo new posts/test.md
```
`content/posts/test.md` は以下のようになっています。

```
---
title: "Test"
date: 2022-11-28T19:25:10+09:00
draft: true
---
```

`draft true`とは，この記事が下書きの状態であることを意味します。したがって，記事を公開したいときには，この行を削除する必要があります。`test.md`に,"Hello World!"という行を追加して，`draft true`の行を削除したものを保存しておきましょう。


### **テーマをインストールする**
---
Hugoでサイトを生成した場合には，外観を決定するためのテーマを設定する必要があります。Hugoの公式サイトに公開されているテーマがたくさんあるので，「かっこいいな」と思ったものを使うとよいでしょう。
(https://themes.gohugo.io/)

今回は，PaperModというテーマを使うことにします。

テーマを追加するには，

```
cd theme
git submodule add git@github.com:adityatelange/hugo-PaperMod.git
```

とコマンドを打ちます。

最後に，追加したテーマの名前を`config.toml`に追加する必要があります。

```
theme = "hugo-PaperMod"
```

これで設定が完了しました！

```
$> hugo
```

というコマンドを打つと，静的サイトの生成がされます。続いて，

```
$> hugo server
```

と打つと，localhostの1313番ポートで静的サイトの配信をします。ブラウザで`http://localhost:1313/`にアクセスしましょう。

![](/images/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202022-11-28%2020.09.12.png)


cool やん?

### **Github Pages でデプロイする**

上の流れで作ったリポジトリを Githubに `<account名>.github.io`という名前で用意したリポジトリに pushして，Github Pages の設定をすると，生成したコンテンツをホスティングしてくれます。[こちらの記事](https://qiita.com/ysdyt/items/a581277dd1312a0e83c3)を参考にして，`config.toml`や，ディレクトリの名前を変更したりしてから，`<account名>.github.io`という名前のリポジトリを作成して，そこに push してからGithub Pages の設定を変更してやってから，`https://<accout名>.github.io`にアクセスすると...

![](/images/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202022-11-29%2015.43.04.png)

よーし，これで静的サイトのホスティングに成功しました！

### **静的サイトジェネレータ + Github Pages + Github Actions = さらにcool**
---
しかし，今のままでは，まだちょっとした問題があります。それは，記事を書いた後にそれを反映するためには毎回`hugo`コマンドを打って，静的サイトを構築し直す必要があることです。せっかく記事を書いたのに，この作業を忘れてしまうと，公開されないままになっていたという状況になります。これはあまりcoolではないですね。

この問題を解決するためには，記事を書くたびに自動で毎回`hugo`コマンドが打たれるような仕組みを用意する必要があります。**Github Actions**を使うことで，この仕組みを用意することができます。

[Github Actions](https://docs.github.com/ja/actions/learn-github-actions/understanding-github-actions)とは,公式サイトによると 『ビルド、テスト、デプロイのパイプラインを自動化できる継続的インテグレーションと継続的デリバリー (CI/CD) のプラットフォームです』とのことです。

よくわからない用語が出てきていますが，要するに，Github上で起きる様々な出来事に反応して，登録しておいた作業が自動的に行われることを可能にする仕組み，という感じなのでしょうか...

### **Github Actions を使ってみる**
よくわからないので，とりあえず使ってみましょう。[公式の手順](https://github.com/marketplace/actions/github-pages-action#getting-started)に従って，`.github/workflows/gh-pages.yml`という構成を追加して,`gh-pages.yml`を以下のように編集しましょう。

```
name: Github Pages

on:
  push:
    branches:
      - main  # Set a branch name to trigger deployment
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    permissions:
      contents: write
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.101.0'

      - name: Build
        run: hugo -minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        # If you're changing the branch from main,
        # also change the `main` in `refs/heads/main`
        # below accordingly.
        if: ${{ github.ref == 'refs/heads/main' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

`gh-pages.yml`の中身を見ると，`on:`というフィールドに書いてあるのが，トリガーとなるイベントの登録であるように見えます。`jobs`に書かれている仕事が，`on:`に書いてあるイベントが起きたときに自動で実行される，というように見えますね。では，`hugo new posts/actions.md`で，テスト用のファイルを作って，push しちゃいましょうか。

そして，Github Pagesの設定から，`gh-pages`ブランチを選択します。

![](/images/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202022-11-30%202.19.26.png)

さて，どうなるかな〜。

![](/images/%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%202022-11-30%202.20.18.png)
もっと，coolになったやん。

### **おわりに**
---
ということで，これでカッコいいサイトができたので，今後はこのサイトを使って42Tokyoのことや，私が勉強したことなんかを載せていこうと思います。今回自分で色々と調べて，一人でサイトを公開することができて，さらに Github Actionsに触れることができて良かったです。まだまだわからないことが多いので，今後も学んでいくことができたらいいな，と思います😊

明日は，@Akky_Orzさんが,記事を書いてくれるようなのでそちらもぜひ読んでくださいね！最後まで読んでいただきありがとうございました！


### **参考リンク**
- [Hugo + GitHub Pages + GitHub Actions で独自ドメインのウェブサイトを構築する.](https://zenn.dev/nikaera/articles/hugo-github-actions-for-github-pages#hugo-%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8B)
- [peaceiris/actions-hugo](https://github.com/peaceiris/actions-hugo)
- [The world’s fastest framework for building websites | Hugo](https://gohugo.io/)
- [HugoとGitHub ActionsでGitHub Pagesを自動デプロイ](https://vicugna-pacos.github.io/hugo/hugo-github-actions/)
- [Hugo と Github Pages でブログを作る - 三日坊主。](https://sat8bit.github.io/posts/hugo-with-github-pages/)
- [Hugo + GitHub Pages（独自ドメイン適応）でサイトを作成・公開する](https://qiita.com/ysdyt/items/a581277dd1312a0e83c3)
