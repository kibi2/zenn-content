---
title: "プロジェクト始動～【連載】実況パズルプログラミング"
emoji: "💨"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["python", "開発", "ポエム", "Eclipse"]
published: true
---
| [<< Back](https://zenn.dev/ogwk/articles/ppb240309blog) | [-- Index --](https://zenn.dev/ogwk/articles/ppb000000contents) | [Next >>](https://zenn.dev/ogwk/articles/ppb240325project) |
| ---- | ---- | ---- |

-----
実況パズルプログラミングの連載第2回目です。 パズルアプリケーションを作成しながら、作成過程で思ったこと、考えたこと、検討したことなどをブログ風に書いていきます。

今回はプロジェクトに着手したところから実況します。

## タスクを書き出す

個人プロジェクトなので思いついたことからどんどん進めて行くのがお手軽で良いです。 ですが一応業務のつもりで進めて行きたいので最初にやるべきタスクを書き出すところから始めます。

- プロジェクト名称を決める
- 開発環境を作成する
- プログラム実装（詳細は[連載第1回目](https://zenn.dev/ogwk/articles/ppb240309blog)を参照のこと）

タスク一覧をどこかで一元管理するのが良いのですが、まだ管理する場所ができていないのでとりあえずここに書いておきます。 管理する場所ができ次第、そちらにまとめたいと思います。

## プロジェクト名称を決める

いろいろと名前を付ける場面が今後出てくるのでプロジェクト名称を決めましょう。 最終的にリリースするときに正式名称を付けることとし、今は仮の名称とします。 なのであまり悩むことなく適当に決めます。 あたりを見回すと「Scottie」という文字が目に入りました。 悪くはないですが、もう少しこだわった方が良いでしょう。 本棚を眺めると魚の図鑑があったので魚の名前を付けることにしましょう。 さかなクンほどではありませんが私は魚が好きです。

プロジェクト名は「okoze」に決定です。

## 開発環境を作成する

次のタスクは「開発環境を作成する」です。 取り掛かる前にタスクを細分化しておきます。

- 開発環境を作成する
    - Git環境作成
    - Python実行環境検討
    - Python実行環境作成
    - IDE検討
    - IDE環境作成
    - プロジェクト作成
    - GitHubとの連携

### Git環境作成

このzennの記事はGitHubと連携して書いているのでGitの環境はすでにあります。 ですが一応スクラッチからやってみたかったので今まで使用していたGitは一度アンインストールしてから再インストールします。

インストール手順を画面付きで詳細に解説することはやめておきます。 私は「git windows インストール」をキーに[Google検索](https://www.google.com/search?q=git+windows+%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB&oq=git+windows+%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)して検索されたサイトを参考にインストールしました。

参考のため私の行った手順を画像抜きで書いておきます。

- [公式サイト](https://git-scm.com/)から[Downloads][Windows][64-bit Git for Windows Setup.]とリンクをたどっていく
- ダウンロードされた「Git-2.44.0-64-bit.exe」を実行する
- 後は画面の指示にしたがってインストール作業を進める
- 私は以下の二点以外はデフォルトの設定としました
    - インストール先を変更する
    - 最初のブランチ名をGitHubに合わせて「main」とする

後はGit Bashを開いて次のコマンドが正常に動作することを確認します。

```bash
$ git -v
git version 2.44.0.windows.1
```

ちなみに私は[ConEmu](https://conemu.github.io/)というソフトからGit Bashを利用しています。

### Python実行環境検討

Pythonでプログラム作成するのでPythonの実行環境を作成します。 Pythonの勉強のため今まではAnacondaを使っていましたがこれも一度アンインストールします。

あらためてPython実行環境について調べてみます。 「python 実行環境」をキーに[Google検索](https://www.google.com/search?q=python+%E5%AE%9F%E8%A1%8C%E7%92%B0%E5%A2%83)です。 お勧めの記事はPythonJapanの「[Python環境構築ガイド](https://www.python.jp/install/install.html)」です。 Pythonを利用するには主に3つの方法があるようです。

- 公式の処理系「CPython」
- Anaconda
- ブラウザから使う「Colab」
- IDE同梱のPythonを使う（これは主でない4番目の項目）

以前Anacondaを使っていたので今回もAnacondaかな、とも思いました。 しかしPythonJapanの記事「[Python環境構築ガイド](https://www.python.jp/install/install.html)」を読んで今回は公式の処理系を使ってみることにします。

### Python実行環境作成

Pythonインストールの前に念のため「アプリと機能」で検索するとPython3.10.4がインストールされていました。 これもいったんアンインストールしておきます。

次にPythonJapanの記事「[Python環境構築ガイド](https://www.python.jp/install/install.html)」にしたがってインストールします。ただし、カスタマイズを選んでインストール先を変更しました。

後はGit Bashを開いて次のコマンドが正常に動作することを確認します。

```bash
$ python --version
Python 3.12.2
```

PythonJapanの記事には仮想環境の使い方も書いてあります。 仮想環境の必要性は分らないので後回しにします。 Cコンパイラのインストールについても書いてありますがCコンパイラは必要ないのでパスします。

### IDE検討

最初はAnacondaについているSpyderを使っていました。 ですが私が使いこなせていないのかもしれませんがデバグ中に変数の値を調べるのが面倒でした。 次に使ったのはVisual Studio Code（以下VSCode）です。 それなりに高機能なのですがちょっと馴染みにくい印象です。 どこがどう馴染みにくいのかは分析できていないので今は説明できず申し訳ありません。

「python ide」をキーに[Google検索](https://www.google.com/search?q=python+ide&oq=python+ide)です。 無料で高機能なIDEとなるとVSCodeかEclipseでしょう。 どちらもWindows, Mac, Linuxで使える点も良いです。 検索では出てきませんがVimをIDEとして使うというのもおもしろそうです。 今回私はJavaの開発でお世話になったEclipseを使ってみたいと思います。 Eclipse+Pythonの使い勝手は分らないのでEclipseが駄目ならVSCodeに戻ります。 落ち着いたら他のIDEも使ってみて比較できたらおもしろそうです。

皆さんはお好きなIDEを使ってください。 特にこだわりがなければ情報量の多いVSCodeか他のIDEを使った方が無難だと思います。

### IDE環境作成

EclipseのインストールにはPleiadesパッケージを使います。 簡単にインストール手順を書いておきます。

- [MergeDoc Project](https://willbrains.jp/#pleiades.html)をブラウザで開く
- 「[Eclipse2023](https://willbrains.jp/index.html#/pleiades_distros2023.html)」のリンクからWindows X64,Pythonをダウンロードする
- ダウンロードした「pleiades-...-jre_20240218.exe」を実行して適当な場所に展開する
- 展開した/pleiades/2023-12/eclipse/eclipse.exeを実行する
- 「ワークスペースとしてのディレクトリー選択」画面で「起動」ボタンを押す
    - 必要に応じてチェックボックスをチェックする
- ファイアウォールの警告画面では「アクセスを許可する」ボタンを押す

以上でインストールと起動ができました。 後は必要に応じてカスタマイズします。

- 以下の記事を読んでマーケットプレイスをインストールする
    - [【Eclipse】マーケットプレイスとは？使い方やインストール方法を解説する](https://miya-system-works.com/blog/detail/117)
- マーケットプレイスを使って好きなプラグインをインストールする
    - Gitを使うので「EGit」をインストールする
    - 私はVimが好きなので「Vrapper」もインストールする
    - ターミナル機能も使いたいので「TMターミナル」もインストールする
- Eclipseの設定（「ウィンドウ」メニューから実行）から外観（「一般」の下層）などを変更する
    - IDEは明るい背景で使う習慣があるので「ルック＆フィール」を「ライト」にする
    - テキストフォントに好みがあれば変更する。私は「Ricty Diminished 12」に変更した

Eclipseの設定からインタープリター（PyDevの下層）を見てみました。 初期設定ではEclilpse同梱のPythonを使うようになっています。 バージョンは3.12.0です。 ここでは前のステップでインストールしたPython3.12.2を使うように変更しました。

結局IDEとしてEclipseを使うのであればPythonとGitのインストールは不要だったのかもしれません。

## 次回に続く

「プロジェクト作成」「GitHubとの連携」が残りました。今回はここまでとします。続きは次回で、またお会いしましょう。

-----
| [<< Back](https://zenn.dev/ogwk/articles/ppb240309blog) | [-- Index --](https://zenn.dev/ogwk/articles/ppb000000contents) | [Next >>](https://zenn.dev/ogwk/articles/ppb240325project) |
| ---- | ---- | ---- |
