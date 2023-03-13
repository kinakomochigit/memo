---
title: LTのスライド作成に時間をかけたくない
tags: Markdown VSCode Marp
author: ta9star
slide: false
---

# きっかけ

いつも LT を行うとき
メモ帳でスライドに書くべき内容をまとめたうえで、パワーポイントを作成していた。
これを"もっと楽にしたい"という思いから本記事に手法をまとめた。
![プレゼンテーション1.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247753/44ad44ea-fbf4-376f-ef80-5d40a5eae5bd.jpeg)

# 今回の目標

.md 形式でかいたらスライドが自動で作成される。
![qiita_marp.md-Visual-Studio-Code-2019-12-23-15-42-26.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247753/be754770-c055-d9f5-fa62-7068d59d4f84.gif)

# 解決策

調べてみるとツールはたくさん存在した。

- reveal.js
- remark.js
- Marp
- Swipe
- Qiita

etc...
参考:[[ Markdown でスライド作成 ] 5 種類くらいツールを試してみた結果](https://qiita.com/ykhirao/items/74a23f812dd5d22b3b88)

---

今回は[Marp](https://marp.app/)を採用した
採用理由は以下

- デザイン性
- 拡張性
- VSCode 上でできる

# Marp でなにができるのか

- md 記法でスライドがかける
- 独自のディレクティブを記述できる
  - ページ番号表示
  - スライドの高さ・幅・縦横比の指定
  - テーマの変更
  - 背景色/文字色等の変更
  - ヘッダー/フッターの表示
  - 背景画像や画像の表示比率等の指定
  - 数式の表示
  - CSS によるカスタマイズ

など

# Marp ってなに？

yhatt さんが作成されているツール。
[GitHub](https://github.com/marp-team/marp)にソースコードが公開されている。

# インストール

今回は VSCode のプラグインである[Marp for VS Code](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)を使用した。

アプリケーション等も用意されているので、VSCode 上でやりたくない人は各自で。

# 使い方

### スライドの確認

.md ファイルを作成 → プレビュー表示
![qiita_marp.md-Visual-Studio-Code-2019-12-23-16-04-38.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247753/a7c3b424-b160-b23f-8340-4b7d27834d62.gif)

### スライドに変換したい

対応フォーマット

- PDF
- HTML
- PowerPoint
- PNG (First slide only)
- JPEG (First slide only)

Marp Command をクリック
![qiita_marp.md-Visual-Studio-Code-2019-12-23-16-07-29.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/247753/9a0354d5-d464-378f-64cd-a4821e7a0c8e.gif)

### テンプレート

```
---
marp: true

---
<!--
theme: default
size: 16:9
paginate: true
style: |
  section {
    background-color: #FFFFFF;
  }
-->
# 1st page

---

## 2nd page

### The content of 2nd page

---

# 3rd page
```

# Directives

Marp は[Directives](https://marpit.marp.app/directives)を使用することで、スライドの作成をサポートできる。

## Global directives

スライド全体の設定値。
同じ Global directives を何度も記述した場合、最後の値のみを認識する。

- テーマの指定
- テーマスタイルの調整
- スライドの分割オプション

---

### テーマの切替

テーマは２種類が選択できる。
カスタマイズすることも可能。

```html:Default
<!-- theme: default -->
```

```html:Gaia
<!-- theme: gaia -->
```

### テーマスタイルの調整

CSS でテーマの調整ができる。

```html:Style
<!--
style: |
  section {
    background-color: #111111;
  }
-->
```

### スライドの分割オプション (HeadingDivider)

見出しの前にスライドページを自動的に分割するように指示できる。

```html:RegularSyntax
# 1st page

---

## 2nd page
### The content of 2nd page

---

# 3rd page
```

```html:HeadingDivider
<!-- headingDivider: 2 -->
# 1st page

## 2nd page

### The content of 2nd page

# 3rd page
```

---

## Local directives

スライドページごとの設定値。定義されたページ及び後続のページに適用される。

- ページ数の表示
- ヘッダーの表示
- フッターの表示
- 背景色の指定
- 文字色の指定

etc...

### 単一のページに適用させる

現在のページにのみ Local Directives を適用する場合は、
directives の名前の前に\_(アンダーバー)をつける。

```html
<!-- _backgroundColor: black -->
```

---

### ページ数の表示

true に設定すると、スライド右下にページ番号が表示される。

```html:Paginate
<!-- paginate: true -->
```

### ヘッダーの表示

スライド左上に任意のヘッダー文字を表示できる。

```html:Header
<!-- header: This is a header -->
```

### フッターの表示

スライド左下に任意のフッター文字を表示できる。

```html:Footer
<!-- footer: This is a footer -->
```

### 背景色の変更

任意の背景色に変更することができる。

```html:BackgroundColor
<!-- backgroundColor: black -->
```

### 文字色の変更

任意の文字色に変更することができる。

```html:Color
<!-- color: white -->
```

---

## その他の機能

### 画像表示

参考:[Image syntax](https://marpit.marp.app/image-syntax)

```html
* 背景画像に設定する ![bg](picture.png) * 画像サイズの比率を設定する
![60%](picture.png) *サイズを自動調整する ![fit](picture.png) *
空白区切りで複数指定 ![bg 60%](picture.png)
```

### 数式の表示

KaTeX 形式で数式を記載することができる。
\$でインライン表示、\$$でブロック表示する。

$$I_{xx}=\int\int_Ry^2f(x,y)\cdot{}dydx$$

```
$$I_{xx}=\int\int_Ry^2f(x,y)\cdot{}dydx$$
```

### スライドのアスペクト比・サイズの指定

スライドのサイズを変更できる。
デフォルトは size: 4:3 で設定されている。

```html
<!-- size: 16:9 -->
```

```html
<!-- width: 12in -->
<!-- height: 6in -->
```

# 終わりに

LT のスライド作成に時間をかけたくない！！
という思いから本記事を書かせていただきました。
これを使用するようになってから
快適な LT ライフを行えるようになったと感じています。
皆様も是非お試しください。

---

### アドベントカレンダーについて

[CA Tech Dojo/Challenge/JOB Advent Calendar 2019](https://qiita.com/advent-calendar/2019/dojo-challenge-job)の 24 日目は
[@ta9star](https://qiita.com/ta9star)が書かせていただきました。
CATechDojo からはじまり、現在は CATechJob をさせていただいています。
インターンシップ期間中はずっと Golang を使った開発ができていて楽しいです。
明日は CATechDojo 仲間の[@yawn*yawn_yawn*](https://qiita.com/yawn_yawn_yawn_)が
面白い記事で締めてくれると思います。
是非見てみてください。
