---
title: "Canvasに画像を貼り付ける～【連載】実況パズルプログラミング"
emoji: "🗂"
type: "idea"
topics: ["javascript", "canvas", "github", "vscode", "okoze"]
published: true
---

いよいよ、okozeそのものを作り始めます。

## 全体の計画

v0.1.0では、数独を扱えるようにします。

このバージョンで予定している機能は次のとおりです。

詳細は [`docs/features.md`](https://github.com/kibi2/okoze/blob/main/docs/features.md) にまとめています。

| 🟧 | ID | Description | Done | Notes |
| -- | -- | --------------------------- | :--: | ----- |
| 🟧 | 1A | 開発環境作成 | 🟢 |  |
| 🟧 | 1B | 簡易プレイ機能 |  |  |
| 🟧 | 1C | 保存、開く |  |  |
| 🟧 | 1D | Undo/redo |  |  |
| 🟧 | 1E | ルールの組み込み |  |  |
| 🟧 | 1F | 定石の組み込み |  |  |
| 🟧 | 1G | ソルバー機能 |  |  |

## 開発の進め方

今回から、機能ごとにブランチを作って開発します。

そして、テストを使って実装の進み具合を記録していきます。

詳しくは [`docs/development.md`](https://github.com/kibi2/okoze/blob/main/docs/development.md) にまとめています。

## 前回から残っているテスト

前回、まだ終わっていないテストが2つあります。

今回はこれには手をつけません。

忘れないように `docs/features.md` に記録してあります。

残っているテストは次の2つです。

> 🟧 1B1 Canvas位置調整
> 🔴 Canvasを利用可能なウィンドウいっぱいに表示できる
> 🔴 ウィンドウサイズが変わったときCanvasのサイズを変更できる

これらは後で戻ってきます。

## 今回やること

今回は、次の機能を実装します。

> 🟧 1B2 イメージの取り込みと表示調整
> 🔴 「貼り付け」を実行すると、クリップボードの内容がCanvasに貼り付けられる
> 🔴 クリップボードに画像がない場合は何も起こらない
> 🔴 画像をCanvasのサイズに合わせて表示できる
>    （画像を自動的に拡大・縮小し、Canvas自体のサイズは変更しない）
> 🔴 画像の縦横比が維持される

順番に実装していきます。

## 🔴 「貼り付け」を実行すると、クリップボードの内容がCanvasに貼り付けられる

### テスト方法

* スクリーンショットを撮ってクリップボードに入れる（Macなら `⌘+Shift+5`）
* okozeをアクティブにして貼り付ける（Macなら `⌘+V`）
* スクリーンショットがCanvasに表示されることを確認する

### 実装

* 「貼り付け」を実行したら、クリップボードの内容をCanvasに表示する
* Canvasの原点に表示する
* 拡大・縮小はしない

まだCanvas APIについては、それほど詳しくありません。

でも、これくらいはやってくれないと困ります。

コードはAIに作ってもらいます。

```javascript
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");
document.addEventListener("paste", (event) => {
    const item = event.clipboardData.items[0];
    const file = item.getAsFile();
    if (file) {
        const image = new Image();
        image.onload = () => {
            ctx.drawImage(image, 0, 0);
        };
        image.src = URL.createObjectURL(file);
    }
});
```

コードの意味がよく分からなかったので、AIにいくつか質問しました。

回答を簡単にまとめると、次のようになります。

* **クリップボードは配列なの？**
  データは種類ごとに格納されるので、複数のitemを持つことがある。
* **`getAsFile()` は何を返すの？**
  スクリーンショットの場合は `File` オブジェクトが返る。
* **Canvasを描き直しても、なぜ画像が消えないの？**
  Canvasは一度描いたピクセルを保持している。
* **クリップボードの仕組みはブラウザやOSによって違わないの？**
  Web APIが違いを吸収してくれる。
* **APIのドキュメントはどこにあるの？**
  [MDN Web API](https://developer.mozilla.org/en-US/docs/Web/API)

テストは通りました。

> 🟢 「貼り付け」を実行すると、クリップボードの内容がCanvasに貼り付けられる

## 🔴 クリップボードに画像がない場合は何も起こらない

テキストをコピーして貼り付けても何も起こりません。

これは `if (file)` のチェックのおかげでしょうか？

念のためAIに確認しました。

どうやら、普通のファイルをコピーした場合も `getAsFile()` が `File` を返すことがあるようです。

私のテストでは、ファイルをコピーして貼り付けても何も起こりませんでした。

しかし、念のため画像かどうかを明示的にチェックすることにします。

```javascript
if (item.type.startsWith("image/")) {
```

これでテストもGreenになりました。

> 🟢 クリップボードに画像がない場合は何も起こらない

## 🔴 画像をCanvasのサイズに合わせて表示できる

AIに実装してもらいます。

```javascript
ctx.drawImage(image, 0, 0, canvas.width, canvas.height);
```

うまく動いているようです。

画像の拡大・縮小のアルゴリズムや、画像の端のピクセルがどのように扱われるのかなど、興味深いところはいろいろあります。

しかし、okozeは画像処理アプリケーションではありません。

今のところ、見た目に問題がなければ十分です。

> 🟢 画像をCanvasのサイズに合わせて表示できる

## 🔴 画像の縦横比が維持される

これもAIに実装してもらいます。

```javascript
const scale = Math.min(
    canvas.width / image.width,
    canvas.height / image.height
);
const width = image.width * scale;
const height = image.height * scale;
ctx.drawImage(image, 0, 0, width, height);
```

うまく動いているようです。

> 🟢 画像の縦横比が維持される

ここで、画像がCanvasの片側に寄っていることに気づきました。そこで、中央に表示することにします。テストを追加します。

> 🔴 画像がCanvasの中央に表示される

## 🔴 画像がCanvasの中央に表示される

これもAIに実装してもらいます。

```javascript
const xPos = (canvas.width - width) / 2;
const yPos = (canvas.height - height) / 2;
ctx.drawImage(image, xPos, yPos, width, height);
```

うまく動きました。

> 🟢 画像がCanvasの中央に表示される

## テスト結果

今回のテスト結果は次のようになりました。

```console
cb0951d 🟧 1B2 Image import and display adjustment
1bbdd52 🟢 When "paste" is executed, the clipboard contents are pasted onto the canvas.
3215af6 🟢 Nothing happens if the clipboard does not contain an image.
48e9e32 🟢 The image is fit to the canvas size (the image is automatically scaled up or down; the canvas itself is not scaled).
58ddef0 🟢 The image's aspect ratio is preserved.
faba506 (HEAD -> feat/paste-image) 🟢 The image is centered on the canvas.
```

今回は、AIにコードを書いてもらいながら、実際に動かして、テストして、必要なところだけ修正しました。

最初から完璧な仕様を決めていたわけではありません。

実際に動かしてみることで、

「画像が片側に寄っている。中央にしたい」

という要求も出てきました。

このように、**作って、動かして、気づいたことはテストに追加する**と進めていきます。

## リンク

[ソースコード](https://github.com/kibi2/okoze/tree/episode-004)
[Live Demo](https://kibi2.github.io/okoze/episode-004/)

## 次回

次は、9×9のマス目を、貼り付けた画像の上に描きます。

> 🟧 1B3 パズルフレーム描画

---

https://zenn.dev/ogwk/articles/ppb000000contents

