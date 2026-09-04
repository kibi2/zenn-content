---
title: "公開済みlive demoを変更する"
emoji: "🐈"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [okoze]
published: false
---

## 公開済みlive demoを変更する

### 動機

okoze本体はJavaScriptの `main.js` です。
`index.html`, `style.css` はlive demo公開のための入れものです。

公開済みのepisodeの `main.js` は、そのepisodeで完成した成果物として原則変更しません。
一方、`index.html`, `style.css` はlive demoの表示や操作性のためのものなので、公開後に修正したくなることがあります。

今回はlive demo公開後に、HTML, CSSだけを変更する仕掛けを作ります。

公開済みのtagそのものは変更しません。
修正した内容には新しいtagを付けることで、元のepisodeの成果物もGitの履歴として残します。

### 作るもの

> 🟧 HTML, CSS変更機能
> 🔴 tag `episode-NNN` の成果物を元に、HTML, CSSを変更できる
> 🔴 `live demo/okoze/episode-NNN/` の内容が変わる
> 🔴 `live demo/okoze/episode-NNN/` のURLは変わらない
> 🔴 live demoのindexは変わらない
> 🔴 その他のlive demo episodeの内容は変わらない

### どうやって使うか

変更したいepisodeのtagを元にbranchを作り、HTML, CSSを変更します。

* 変更したいepisodeのtagをcheckout
* branch `fix/episode-NNN-fix1` を作成
* HTML, CSSを変更
* commit, push
* tag `episode-NNN-fix1` をつける
  * `fix` に続く1文字を修正番号とし、`1`～`9`, `A`～`Z`, `a`～`z` の順に使用する
* tagをpush
  * GitHub Actionsが実行され、live demoが更新される
* branch `fix/episode-*-fix1` を削除

例えば `episode-003` を修正する場合は、次のようにします。

```bash
git switch --detach episode-003
git switch -c fix/episode-003-fix1

vi index.html

git add index.html
git commit -m "Fix episode-003 live demo"
git push -u origin fix/episode-003-fix1

git tag -a episode-003-fix1 -m "episode-003-fix1"
git push origin episode-003-fix1

# live demo が更新される

git switch main
git branch -D fix/episode-003-fix1
git push origin --delete fix/episode-003-fix1
````

### 実装内容

GitHub Actionsを変更します。

* すべてのtag（`episode-*`）をアルファベット順に処理する
* tagをcheckoutする
* tag名からepisode番号を取り出す
* `episode-NNN/` にdeployする
* 同じepisode番号のtagが後から処理された場合は、以前の内容を削除して作り直す

例えば、

```text
episode-003
episode-003-fix1
episode-003-fix2
```

の順に処理されます。

`episode-003-fix1` を処理すると、`episode-003/` は `episode-003-fix1` の内容で作り直されます。
さらに `episode-003-fix2` を処理すると、`episode-003-fix2` の内容で作り直されます。

そのため、live demoのURLは

```text
/okoze/episode-003/
```

のまま変わりませんが、最終的には最新のfixの内容が表示されます。

### Links

[Source code](https://github.com/kibi2/okoze/blob/35b596485a2947ecee174ec2abba4e90fd861301/.github/workflows/deploy-pages.yml)
