---
title: "pushd のディレクトリスタックを共有する"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [linux]
published: false
---
Zenn を GitHub と連携し、bash と vim を使って記事を書いています。
久しぶりにシェルと vim を使うのでいろいろと思い出して時間がかかります。
そう言えば pushd とか使っていたな、とか。.bashrc にこんな感じで書いていました。

```bash
alias	pwd='dirs -v'
alias	pd='pushd'
alias	pd1='pushd +1'
alias	pd2='pushd +2'
alias	pd3='pushd +3'
alias	pd4='pushd +4'
alias	pd5='pushd +5'
alias	pd6='pushd +6'
```

ところで昔は複数の端末を開いてその間でディレクトリスタックを共有していたと思いましたが、その方法が分かりませんでした。
色々調べてやっと分かったのがこれです。

[tcsh の dirs 組み込みコマンド : ディレクトリー・スタ ックを出力する](https://www.ibm.com/docs/ja/zos/2.4.0?topic=tics-dirs-built-in-command-tcsh-print-directory-stack)

これによると
```tcsh
dirs -S # ディレクトリスタックをファイルに書き込む
dirs -L # ディレクトリスタックをファイルから読み込む
```

tcsh のコマンド特有のオプションみたいです。そういえば昔は tcsh を愛用していました。
