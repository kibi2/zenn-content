---
title: "プロジェクト作成～【連載】実況パズルプログラミング"
emoji: "👻"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["python", "開発", "ポエム", "Eclipse"]
published: false
---
[前回の記事](https://zenn.dev/ogwk/articles/ppb240319py_install)

-----
実況パズルプログラミングの連載第3回目です。 パズルアプリケーションを作成しながら、作成過程で思ったこと、考えたこと、検討したことなどをブログ風に書いていきます。

今回はプロジェクトのディレクトリ作成からです。

## プロジェクト構成を決める

今までJavaのプロジェクトでは最低以下の三つのディレクトリを作成していました。

- src
- doc
- test

さて今回はPythonで開発するのでPythonの流儀に従うのが良さそうです。
早速「Python ディレクトリ構成」で[Google検索](https://www.google.com/search?q=python+%E3%83%87%E3%82%A3%E3%83%AC%E3%82%AF%E3%83%88%E3%83%AA%E6%A7%8B%E6%88%90)します。
色々な人が色々なことを述べていて何を採用してい良いのかはっきりしません。
また初心者なのでパッケージ、モジュールとかも良く分かっていません。

比較的分かりやすかった次のサイトを参考にディレクト構成を決めます。

[【図で解説】Python アプリケーション推奨のフォルダ構成（ディレクトリ構成）](https://resanaplaza.com/2021/07/17/%E3%80%90%E5%9B%B3%E3%81%A7%E8%A7%A3%E8%AA%AC%E3%80%91python-%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E6%8E%A8%E5%A5%A8%E3%81%AE%E3%83%95%E3%82%A9%E3%83%AB%E3%83%80/)

- okoze （プロジェクトトップ）
    - okoze （パッケージ）
        - \_\_init\_\_.py
        - \_\_main\_\_.py （モジュール）
    - docs
    - tests

## プロジェクトを作成する

それではプロジェクトを作成しましょう。
Eclipseを起動し以下の手順でプロジェクトを作成します。

1. メニューから[ファイル][新規][PyDevプロジェクト]を実行
    - プロジェクト名を「okoza」にして「完了」ボタンを押す
1. メニューから[ファイル][新規][PyDevパッケージ]を実行
    - ソース・フォルダーが「/okoze」になっていることを確認する
    - 名前を「okoza」にして「完了」ボタンを押す
1. メニューから[ファイル][新規][PyDevモジュール]を実行
    - ソース・フォルダーとパッケージを確認
    - 名前を「\_\_main\_\_」にして「完了」ボタンを押す
    - テンプレートは「モジュール:メイン」を選択して「OK」ボタンを押す
1. メニューから[ファイル][新規][フォルダー]を実行
    - プロジェクト直下に「docs」フォルダー作成
1. 同様に「tests」フォルダーを作成

以上の操作で次の様になりました。

![](https://i.gyazo.com/1f2ac9cc5ad6dcf2eefa469c3c10d1fa.png)

「\_\_init\_\_.py」は手順2で自動的にできます。
「Python3」は手順1で自動的にできます。
「\_\_main\_\_.py」の「@author: OGAWA Keiji」の部分はWindowsのログインユーザー名になるので変更しました。
自動で変更したい場合は「eclipse.exe」と同じディレクトリにある「clipse.ini」に次の行を追加してください。

```
-Duser.name=OGAWA Keiji
```

## プロジェクトを実行する

実行確認できるように\_\_main\_\_.pyに次の一行を追加して保存します。

```python
    print('Hello, world!')
```

「okozeモジュール」で右クリックしてポップアップメニューから[実行][Python実行]を実行します。
上手くいけばコンソール画面に「'Hello, world!'」と表示されます。

先ほど参考にしたサイトには次の様に実行する方法が書いてあります。

```sh
Python -m プロジェクト名
```

「-m」オプションを付けないと「\_\_init\_\_.py」が実行されないそうです。
コンソールを開いて実験してみたところまさしくその通りでした。
Eclipseでは上記の方法で実行すると「\_\_init\_\_.py」が実行されませんでした。
そこでメニューの「実行」「実行構成」を開いて次の様に引数を変更すると「\_\_init\_\_.py」が実行されるようになりました。

![](https://i.gyazo.com/bbf6754465633f4f058872aaa7861ace.png)

ついでに名前を変えてお気に入りに入れました。

![](https://i.gyazo.com/d91ebf64de375530417b0b9e4568f959.png)

### 次回に続く

「GitHubとの連携」が残りました。今回はここまでとします。続きは次回で、またお会いしましょう。

-----

[前回の記事](https://zenn.dev/ogwk/articles/ppb240319py_install)