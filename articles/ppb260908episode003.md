---
title: "開発環境を作る～【連載】実況パズルプログラミング"
emoji: "📘"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["javascript", "canvas", "github", "vscode", "okoze"]
published: true
---

前回は、okozeの開発環境を決めました。

今回は、いよいよ実際に開発を始めます。

まずは開発環境を構築して、ちゃんと動くことを確認します。

そして、もう一つ大事なことがあります。

前々回で説明した**開発プロセスそのものを、ここから実際に使ってみます。**

## 今回実装するもの

今回やることを書き出します（`1A` は機能IDです。）

> 🟧 1A 開発環境を作る


作業を始める前に、完成したかどうかを判断するためのテストを書いておきます。

> 🔴 VSCodeでプロジェクトを編集できる
> 🔴 Web サーバーを起動して、ブラウザにCanvasと図形を表示できる
> 🔴 GitHubのリポジトリでプロジェクトの内容を見ることができる

okozeの開発を始めるために必要な最低限の環境を作ることが目的です。

## 開発環境

前回説明した開発環境を使います。

| 目的               | ツール             |
| ------------------ | ------------------ |
| 開発マシン         | Mac mini M4        |
| IDE                | Visual Studio Code |
| ブラウザ           | Chromium           |
| 開発サーバー       | Live Server        |
| プログラミング言語 | JavaScript         |
| グラフィックス     | Canvas API         |
| バージョン管理     | Git / GitHub       |

参考までに、私の現在の環境は次のようになっています。

* macOS: Tahoe 26.5.2
* Visual Studio Code: 1.133.0
* Chromium: 151.0.7922.137
* Git: 2.50.1

Visual Studio Codeではいくつか拡張機能も使っていますが、okozeの必須条件ではありません。

読者の方は、それぞれ使いやすい拡張機能を使えばよいと思います。

OS、IDE、ブラウザについても同様です。

ここでは、できるだけ特定の環境に依存しないように進めます。

## リポジトリを作る

まず、GitHubに空のリポジトリを作ります。

そして、それを開発マシンにcloneします。

この時点では、まだアプリケーションのコードは何もありません。

## 最初のページを作る

cloneしたプロジェクトをVisual Studio Codeで開きます。

そして、次の3つのファイルを作ります。

```text
index.html
main.js
style.css
```

フレームワークは使いません。ビルドシステムもありません。

最初の `index.html` にはCanvasを置き、`main.js` を読み込ませます。

`main.js` ではCanvasのコンテキストを取得して、単純な図形を描きます。

`style.css` には、最低限必要なCSSだけを書きます。

最初のコードは次のようになりました。

```html:index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>okoze</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <canvas id="canvas" width="400" height="300"></canvas>
  <script src="main.js"></script>
</body>
</html>
```

```js:main.js
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

ctx.fillRect(50, 50, 300, 200);
```

もともとAIが提案したコードでは、長方形の**輪郭だけ**を描いていました。

しかし、それではCanvas自身の枠線と区別しにくいため、塗りつぶした長方形に変更しました。

```css:style.css
canvas {
  border: 1px solid black;
}
```

こちらも、もともとAIが提案したコードにはCanvasの枠線がありませんでした。

Canvasがどこにあって、どのくらいの大きさなのかがすぐ分かるように、枠線を追加しました。

**AIにコードを書いてもらうことと、そのコードをそのまま採用することは別です。**

AIにコードを書いてもらい、結果が本当に自分の意図に合っているかを自分で判断します。

:::message
この連載で紹介するコードは、必ずしもAIが最初に提案したコードそのものではありません。

生成されたコードが自分の意図した動作をしなかったり、理解しにくかったりした場合には、自分で変更します。
:::

さて、ここで確認したいのは、

**Visual Studio Codeを使ってプロジェクトを作成・編集できるか**

ということです。

確認できました。テスト項目は赤から緑に変更です。

> 🟢 VSCodeでプロジェクトを編集できる

ここでcommitします。

今回が最初のcommitなので、まずGitの履歴に「この機能を開始した」という記録を残します。

```bash
git commit --allow-empty -m "🟧 1A Development environment"
git add -A
git commit -m "🟢 The project can be edited in VSCode"
```

最初のcommitが、機能の開始を示します。

その次のcommitが、最初のテストがGreenになったことを示します。

## 開発サーバーを起動する

次に、Visual Studio CodeからLive Serverを起動します。

アプリケーションがChromiumで開きます。

ブラウザにはCanvasが表示され、その中に `main.js` で描いた長方形が表示されました。

これで2番目のテストもGreenです。

テストそのものをcommit messageにしてcommitします。

> 🟢 Web サーバーを起動して、ブラウザにCanvasと図形を表示できる

ここで、別のことに気がつきました。

現在のCanvasは固定サイズです。

最終的には、Canvasをウィンドウいっぱいに表示したり、ウィンドウサイズの変更に応じてCanvasのサイズを変更したりしたくなるでしょう。

そこで、新しいテストを2つ追加します。

> 🔴 Canvasを利用可能なウィンドウいっぱいに表示できる
> 🔴 ウィンドウサイズが変わったときCanvasのサイズを変更できる

という要求です。

もちろん、ここで実装してしまうこともできます。

しかし、**今回は実装しません。**

今実装している機能は、これです。

> 🟧 1A 開発環境を作る

新しく見つかった要求は、開発環境ではなく、Canvasのレイアウトや動作に関するものです。

したがって、今は要求を記録するだけにします。

:::message
 開発中に新しい要求を見つけたからといって、それをすぐに実装する必要はありません。
 その要求を記録しておけば、忘れてしまうことを防げます。
 気がついたことをその場ですべて実装し始めると、やることがどんどん増えて、今回で実装が終わって連載が終了してしまいます。
 今回は、これらのテストを赤のまま残します。
:::

## プロジェクトをcommitする

最後に、プロジェクトをGitHubのリポジトリへpushします。

これで、GitHubのリポジトリからプロジェクトのファイルを見ることができるようになりました。

> 🟢 GitHubのリポジトリでプロジェクトの内容を見ることができる

これもcommitします。

これで、最初に書いた3つのテストはすべてGreenになりました。

一方、途中で追加した2つのテストは、まだRedです。

> 🔴 Canvasを利用可能なウィンドウいっぱいに表示できる
> 🔴 ウィンドウサイズが変わったときCanvasのサイズを変更できる

この2つは、必要になった時に実装することにします。

## 最初の機能が完成した

これで、「🟧 1A 開発環境を作る」が完成しました。

また、前々回で説明した開発プロセスを、実際に一通り使うことができました。

```text
小さな仕様を書く
        ↓
テストを書く
        ↓
実装する
        ↓
Red → Green
        ↓
commitする
        ↓
pushする
```

開発環境そのものも、必要なものだけに限定しています。

* ソースコードを編集する
* ブラウザで実行する
* Canvasに何かを描く
* commitする
* GitHubでプロジェクトを見る

これだけです。

**okozeの開発を始めるには、これで十分です。**

そして、今回もう一つ重要なことがありました。

開発途中で、新しい要求を2つ見つけました。

しかし、それらをすぐには実装せず、テストとして記録してから、もともと実装していた機能の完成を優先しました。

これによって、1つのエピソードで扱う範囲を小さく保つことができます。

まだ、肝心のパズルアプリはほとんど始まっていません。

しかし、次のエピソードからは、この開発環境を使っていよいよokozeそのものを作っていきます。

## ソースコード、Live Demo、開発プロセス

今回のソースコードには、

```text
episode-003
```

というタグを付けています。

> **Code:** [episode-003](https://github.com/kibi2/okoze/tree/episode-003)

Gitのタグをpushすると、GitHub Actionsによって、そのタグに対応するソースコードがGitHub Pagesへ公開される仕組みにしています。

したがって、今回のエピソードのLive Demoも見ることができます。

> **Live demo:** [okoze episode-003](https://kibi2.github.io/okoze/episode-003/)

この仕組みについては、Wikiの[Publishing an Episode as a Web App with GitHub Pages](https://github.com/kibi2/okoze/wiki/wiki-tools-001)で説明しています。

:::message
Live Demo用に、`index.html` にはタイトルなどの説明情報を追加しています。
そのため、Live Demoのコードは、開発時の最初のコードと完全に同じではありません。
:::

## リポジトリを少し整理する

最後に、オープンソースプロジェクトとして必要になる2つのファイルを追加します。

```text
README.md
LICENSE
```

ライセンスにはMIT Licenseを使用します。

これらはアプリケーションそのものではありません。

特に面白い話でもないので、ここでは詳しく説明しません。

ただ、今後開発を続けていく前に、リポジトリに最低限の説明とライセンスを用意しておきます。

## Gitの履歴を見る

今回、機能やテストをcommit messageとして記録しているのには理由があります。

`git log` を見ると、どのように開発が進んだのかが分かるからです。

今回の最終的な履歴は、おおよそ次のようになりました。

```bash
git log --oneline --reverse
```

```console
ffce7aa 🟧 1A Development environment
75425a3 🟢 The project can be edited in VSCode
e6b2ad9 🟢 The Web Server can be started and the Canvas and a shape can be displayed in the browser
a8e76d1 🟢 The project contents can be viewed in the GitHub repository
ee693a0 (HEAD -> main, origin/main) doc: Add README and MIT license
```

もちろん、実際のcommit IDは異なります。

ここで面白いと思っているのは、**Gitの履歴そのものが、小さな開発記録になっていること**です。

```text
🟧 feature
   ↓
🟢 test
   ↓
🟢 test
   ↓
🟢 test
```

コードだけが開発の記録ではありません。

Gitの履歴を見ることで、

* どの機能を実装していたのか
* どのテストを確認したのか
* どの順番で完成していったのか

も見えてきます。

今回は非常に小さな例ですが、これからのokozeの開発でも、この方法を続けていきたいと思います。

次回からは、いよいよパズルそのものを作り始めます。

---

https://zenn.dev/ogwk/articles/ppb000000contents