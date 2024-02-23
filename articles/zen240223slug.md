---
title: "Zennのslugについて"
emoji: "📑"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [zenn]
published: true
---
Zennに投稿するときデフォルトのslugではファイル名が無意味なので管理できない。

なので自前でslugを指定する必要がある。

方法は「[Zennのスラッグ（slug）とは](Zennのスラッグ（slug）とは)」に解説がある。

疑問点は

- 「他ユーザーの記事で使用済みのslugも使用できないのでご注意ください。」となっているが、重複するとどうなるの？
- オプション「--slug what-is-slug」でスラッグを指定するようだが、オプション無しで作成して自前で名前変更しても良いか？
  
## 他ユーザーの使用済みslugを使用するとどうなる？

実験してみたところZennでのデプロイで失敗する。
![](/images/etc/zenn-slug-error.png)
失敗するタイミングが遅いので最初から重複しないslugを付ける必要がある。
slugの要件は
- 他ユーザー込みでユニークであること
- [a-z0-9_] で12-50字
- 原則変更不可

自分で規則を作るしかないか。

## slug命名規則

`ctg240223xtitleZ`

- ctg：最初にカテゴリの略称（何文字でも良いが3文字にしておく）
- 240223：作成日付
- x：枝番、最初はなし、その後1,2,3と振る
- title：記事名、アルファベットで始めること（3文字以上）
  
こんな感じでしょうか。こうすればファイル名ソートでカテゴリ順、日付順、枝番順に並べることが期待できる。
絶対重複しないという保証はないが、ほぼ重複はないと期待できそう。
でも多数の人が真似しだすと困ったことが発生するかもしれないが杞憂であろう。

## 重複した名前なのでファイル名称変更する

ファイル名称を変更してpushしたところうまく出来ました。
なので「オプション無しで作成して自前で名前変更しても良いか？」に対する答えも「良い」みたいだ。