---
title: "9x9のマス目を描く-その1"
emoji: "🐕"
type: "idea" # tech: 技術記事 / idea: アイデア
topics:
  - "javascript"
  - "開発"
  - "vscode"
  - "github"
  - "ai"
published: true
---

前回まではCanvasを配置して、ナンプレの画像をCanvasに貼り付けるところまで実装しました。

少し使ってみると、足りない機能に気づきました。現在はCanvasに画像を何枚でも貼り付けることができます。新しい画像を貼り付ける場合は、元の画像をクリアしないといけません。

また、古い画像を使って作業中であれば、クリアしてよいか確認する必要があります。

この機能が必要なので、機能一覧に項目を追加しておきます。

&#128999; 1Cx 新規ゲーム作成機能

内容をどうするかは、まだ決まっていません。今はまだ実装しません。忘れないように機能一覧に追加しておきます。

## okozeとcanvasの分離 

前回はindex.html、style.css、main.jsの三つのファイルを作りました。

このうちmain.jsがokoze本体です。index.htmlとstyle.cssは、okozeをWebページに貼り付けて使うためのサンプルです。

okozeは特定のHTMLページに依存するものではなく、Canvasを使って動くものにしたいと思っています。

HTMLには複数のCanvasがあるかもしれません。そこで、Canvasとokozeを結びつけるため、Canvasにid="okoze"を付けることにします。

```html
<canvas id="okoze" width="400" height="300"></canvas>
```

```javascript
const canvas = document.getElementById("okoze");
```

これで、HTML側のCanvasとokoze本体を分離できます。

テストを追加して動作確認します。

> 🟧 1B1 Canvas位置調整
> 🟢 id="okoze" がついたcanvasにだけokozeが動作する

やり残したテスト項目もokoze本体ではないので、プロジェクトの[`docs/todo.md`](https://github.com/kibi2/okoze/blob/main/docs/todo.md)に移動します。

> 🔴 Canvasを利用可能なウィンドウいっぱいに表示できる
> 🔴 ウィンドウサイズが変わったときCanvasのサイズを変更できる

## &#128999;1B3 9x9のマス目を描いて配置する

今回はこの機能を実装します。二つの機能に分けることができます。
- 9x9のマス目を描く
- マス目をナンプレ画像の枠に一致させるようにマス目を移動・配置する

その前にCanvasを使って何ができるか、概要を把握しておきましょう。
"Canvas javascript チュートリアル" で検索してヒットした次の資料に目を通しておきます。

[キャンバスのチュートリアル](https://developer.mozilla.org/ja/docs/Web/API/Canvas_API/Tutorial)

思った以上にそこそこのことができそうです。頼もしいです。

## イメージの上に矩形(正方形)を描く

&#128308;イメージの上に矩形(正方形)を描く(一辺の長さは画像の短辺の8割、センタリング) 

これが最初にクリアすべきテスト項目です。
9x9のグリッドの前に外枠の矩形だけ描くことにします。

:::message
図形処理の世界では長方形と呼ばずに矩形と呼びます。なぜでしょう？AIに聞いてみました。

**A. 「長方形」だと縦長の図形を連想しやすいのに対し、「矩形」は正方形も含めた一般的な長方形を指す専門用語として扱いやすいからです。**
:::

そして次に矩形をどう表現するか決めないといけません。
okozeの開発はオブジェクト指向で行うことにします。ですので矩形のクラスを作ります。

## 矩形クラスを設計する

ここはいろいろ細々した話を並べ立てているので興味のない方は次へ進みましょう。

クラスの名前はBox2dにしましょう。形状はどう持たせるとよいでしょう。案を考えましょう。

- 案1: 原点座標と幅、高さを持つ
- 案2: 原点座標とその対角位置の座標を持つ
- 案3: X軸方向の最小最大値と、Y軸方向の最小最大値を持つ

座標値が整数だけであればこれらの案に大した違いはありません。ですが座標値が実数になると違いができてきます。
その違いはなんだかわかりますか？

コンピュータで実数計算すると桁落ちが問題になることがあります。値がほぼ等しい実数の引き算で有効桁数が大幅に減少する現象です。興味のある方は「桁落ち」などで調べてみてください。

詳しくは説明しませんが、案2では図形を移動すると、2点から計算する幅や高さがわずかに変化することがあります。案1では、対角上の点の座標を原点の座標と幅・高さから計算する必要があります。そのとき、もともと一致していた点の座標が一致しなくなる、といったことが起きます。

数学上のモデルをそのまま計算機上に作り上げるのには限界があります。幅や高さのわずかな変化は、今回は目をつぶることにしましょう。一方、もともと一致していた点がズレてくるのは少し問題なので、案2を採用することにします。移動するときに座標値を書き換えるのではなく、座標変換行列を使うようにすれば、こうした問題をある程度回避できます。

点のズレを防ぎたい場合は、点座標を直接持つのではなく、点オブジェクトを共有する方法もあります。今回はここまではやりません。

なお、案2と案3は表現している情報としては同じです。今回は、2点の位置を覚えるという実装にしたかったので、案2を採用しました。

座標値を持つクラスも作ります。名前はVec2d(2次元ベクトル)です。座標値は位置ベクトルと解釈できるのでベクトルを使います。持つ値はX座標値とY座標値です。

座標系も決める必要があります。この手のパズルは左上が原点で、X軸は右方向、Y軸は下方向です。
スプレッドシートの座標系と同じです。Canvasの座標系も同じなので、そのまま使います。

## class Vec2d を作る

```js
export class Vec2d {
    constructor(vx, vy) {
        this.vx = vx
        this.vy = vy
    }
    getX() {
        return this.vx
    }
    getY() {
        return this.vy
    }
}
```

AIに作ってもらいました。AIの案は、CVec2d.x, y でしたが修正しました。
なぜ修正したのか話すと長くなるので別途コラムにして解説します。

この手のクラスのメンバーX座標、Y座標はpublicが手軽でよいのですが、privateにしてアクセサーgetX(), getY()を作ります。
基本的にすべてprivateにしてアクセサーを作る方針で進めます。破綻した時また考え直します。

## class Box2d を作る

```js
import { Vec2d } from "./vec2d.js";
export class Box2d {
    constructor(point1, point2) {
        let minX = Math.min(point1.getX(), point2.getX())
        let maxX = Math.max(point1.getX(), point2.getX())
        let minY = Math.min(point1.getY(), point2.getY())
        let maxY = Math.max(point1.getY(), point2.getY())
        this.min = new Vec2d(minX, minY)
        this.max = new Vec2d(maxX, maxY)
    }
    getMin() {
        return this.min
    }
    getMax() {
        return this.max
    }
}
```

対角の2点をからBox2dを作ります。2点の順序は関係ないです。
内部は対角の2点をmin, maxとして持ちます。座標の小さい方をmin, 大きい方をmaxに設定します。
矩形の角は4個ありますが、どれをメンバーとして覚えるかに意味は持たせたくないので覚える位置を固定しています。
getMin() と getMax() では内部のVec2dをそのまま返しています。このあたりはまだ設計を固めていません。

## 描画クラスを作る

canvasに描画しますが、ここは一つ間に描画クラスgraph2dを挟むことにします。
こうしておけばズーム、パンとか言ったカメラ操作もここで全部吸収できるので便利です。

```js
export class Graph2d {
    constructor(ctx) {
        this.ctx = ctx
    }
    draw(box) {
        let min = box.getMin()
        this.ctx.strokeRect(
            min.getX(),
            min.getY(),
            box.getWidth(),
            box.getHeight()
        )
    }
}
```

Graph2dはjavaのGraphics2Dを参考にしています。drawの引数は今はBox2dしか受け取りません。将来classが増えたら種類に応じて処理を分ける予定です。

Box2dにも関数を追加したので実装しておきます。

```js
export class Box2d {
    getWidth() {
        return this.max.getX() - this.min.getX()
    }
    getHeight() {
        return this.max.getY() - this.min.getY()
    }
    draw(graph) {
        graph.draw(this)
    }
}
```

## main.js を完成させる

main.jsにBox2dを作って描画するように処理を入れます。

```js
import { Vec2d } from "./vec2d.js";
import { Box2d } from "./box2d.js";
import { Graph2d } from "./graph2d.js";

const canvas = document.getElementById("okoze");
const ctx = canvas.getContext("2d");
const graph2d = new Graph2d(ctx);

const boxSize = Math.min(canvas.width, canvas.height) * 0.8;
const box = new Box2d(
    new Vec2d(
        (canvas.width - boxSize) / 2,
        (canvas.height - boxSize) / 2
    ),
    new Vec2d(
        (canvas.width + boxSize) / 2,
        (canvas.height + boxSize) / 2
    )
);

box.draw(graph2d);
```

## test

これで実行するとエラーが出ました。

```text
main.js:1 Uncaught SyntaxError: Cannot use import statement outside a module
```

AIに相談してindex.htmlで次のようにtypeを付け加えました。

```html
<script type="module" src="./main.js"></script>
```

実行しましたが矩形の上にイメージを描いてしまいます。
イメージを描いた後でも矩形を描画するように変更しました。

```js
ctx.drawImage(image, xPos, yPos, width, height);
box.draw(graph2d);
```

テストが通りました。実装完了です。

🟢 イメージの上に矩形(正方形)を描く(一辺の長さは画像の短辺の8割、センタリング) 

[Live Demo](https://kibi2.github.io/okoze/episode-005/)

## 次回 

後半の枠を移動する処理が残ってしまいました。

次回は今回作成した矩形の位置をナンプレの枠に一致するように移動する処理を作ります

## Links 

https://github.com/kibi2/okoze/tree/episode-005
https://zenn.dev/ogwk/articles/ppb000000contents

