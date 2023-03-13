---
title: 【VS Code + Marp】Markdownから爆速・自由自在なデザインで、プレゼンスライドを作る
tags: VisualStudioCode Marp Draw.io vega Markdown
author: tomo_makes
slide: false
---

> **応用** VSCode + Marp で作ったデッキのプレゼン方法をアレンジしてみました
> :point_right: [【ブラウザだけ、Zoom/ Teams ほかで動く】「mmhmm」のようなプレゼン+人物動画リアルタイム合成を自作してみる - Qiita](https://qiita.com/tomo_makes/items/17dc2732f042238db419) > ![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/58c77177-b979-b1fa-60b6-00cff52179ca.png)

# TL;DR

Visual Studio Code 上で、Markdown から、こんな感じのデッキを生成できるようにします。
使用したファイル類は、GitHub [tomo-makes/marp-styles](https://github.com/tomo-makes/marp-styles) にまとめました。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/b8f21f44-bd78-c216-fc18-9c9acf4a6d07.png)

# きっかけ

- 叩き台となる資料がなく、急ぎプレゼンをする機会があり、Marp で作成した
- 内輪では使っていたが、多くの目に触れるのは初めてで、もう少しデザインを調整したいと思った
- 今後も使いまわせるものを、スニペット、およびサンプルテーマ化しておこうと思い立った
- ついでにいろいろな図表の生成とデッキへの入れ方、必要そうな配色、素材のリンクをまとめておきたい

# Marp とは

> [Marp: Markdown Presentation Ecosystem](https://marp.app/)

Markdown から、プレゼンスライドを生成してくれるツールです。
当初 Electron 製アプリとして開発されました。その後、より汎用的に使える、モジュラーな設計の[Marp Next としてリリースされました](https://tech.speee.jp/entry/2019/08/01/173843)。
私自身、Reveal.js、GitPitch などいくつか試した上で、Markdown を書く、プレゼンをする、配布資料化する、などを考えた時に、現状 Marp ecosystem + VSCode が最も手に馴染んでいます。Marp の頃から、継続開発に感謝です。

Markdown からのスライド生成には、以下のような利点があります。

- 一定品質のスライドがすぐできる
  - 普段使いのエディタで、箇条書きをするだけでスライドになる
- 既存文書が再利用できる
  - なんなら既存文書にディレクティブを加えるだけで、デッキになる
- デザインとコンテンツを分離できる
  - 文書作成に集中できる
  - 調整が容易、かつ諦めがつく
- Git などでバージョン管理ができる
  - 図表等難しい部分はありますが、大枠はトラッキングできます

ドキュメントは以下に充実しています。詳しい機能はそこを参照いただくのが良いです。

- [marp-team/marp-vscode: Marp for VS Code: Create slide deck written in Marp Markdown on VS Code](https://github.com/marp-team/marp-vscode)
- [Marpit Markdown - Marpit: The skinny framework for creating slide deck from Markdown](https://marpit.marp.app/markdown)

Marp は、Markdown からのプレゼンスライド作成のシーンで、VS Code は他のプラグインと組み合わせ、ざっくりこんな風に使えます。以降、セットアップから使い方を紹介します。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/92401e50-9a95-ccc2-c1a6-697fa3bf2f18.png)

# 事前準備

Marp for VS Code プラグインを、お手元の VSCode に追加しておきます。

> [Marp for VS Code - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode)

# 進め方

ここからは、以下のように進めていきます。

- **ウォーミングアップ**
  - はじめてのスライド作成からプレビュー、エクスポートまで
- **全スライドに適用する**
  - テーマを変えてみる
- **一部スライドに適用する**
  - スライド一枚、または特定のスライド以降にスタイルを適用する
- **画像をいい感じに扱う**
  - 画像を背景、左右に表示したり、エフェクトを適用する
- **カスタムテーマを使う**
  - カスタムテーマ(CSS)を作り、自由自在にデザインする
- **Kroki や Draw.io の図表、Vega のグラフを使う**
  - 関連リソースや実行例をまとめる

# ウォーミングアップ

## はじめての Marp スライド作成

簡単なデッキを作り、プレビュー、配布資料エクスポートができることを試します。
まず、`base.md` という名前で Markdown を作成、保存しましょう。

```html
---
marp: true
---

# タイトル --- # スライド1 テスト --- # スライド2 テスト ---
```

## 既存 Markdown を有効活用したい時は

実は、 `---` は必須ではなく、 `headingDivider: 1` を記載すれば等価で、以下のようにも書けます。

```html
---
marp: true
---

<!--
headingDivider: 1
-->

# タイトル # スライド1 テスト # スライド2 テスト
```

この数字は、1 ならば `#` の見出しごとに自動的にスライドを分けてくれます。 2 ならば `##`、 3 ならば `###` で区切られることになります。
元々プレゼン向けに書いたわけではない Markdown から、スライド投影、発表を急遽行いたい時に便利です。

## プレビューから PDF エクスポートまで

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/1e551b58-86f3-f946-f89e-8a09f8e887c3.png)　のボタンを押し、プレビューをしてみます。

すると、右ペインにスライド一覧が出ます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/6386d680-3545-dbc8-7be0-7f416f8ce09d.png)

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/5a57f4ec-1841-27d4-aedc-4d782ea84eeb.png)　左ペイン Marp アイコンを押します。`Export slide deck...` を選択し、配布資料エクスポートをしてみます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/e8163645-2f2e-d54c-72c4-52b5061990be.png)

エクスポートが完了すると、 `base.pdf` として保存され、プレビューが開きます。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/eebe4c59-8064-9edf-10e7-c959c38a5ece.png)

# さまざまなスタイル変更

## 全スライドに適用 - Global directives

`theme: gaia` に変更し、header, footer を加えてみます。

```html
---
marp: true
theme: gaia
header: "**Qiita** __Marp samples__"
footer: "by **＠tomo-makes**"
---

# タイトル --- # スライド1 テスト --- # スライド2 テスト ---
```

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/5bd36edd-00ee-1719-5906-5e01fc4bb408.png)

## 一部スライドに適用 - Local directives

- `theme: uncover` に変更し、各スライドだけに適用する指定をします。
- `<!-- xxx -->` の形式で、各スライドの頭に設定を記述しています。
- `_(アンダースコア)` が頭についている場合、「対象のスライドのみ」に設定を適用します。
- `_(アンダースコア)` をつけない場合、「同スライド以降の全て」に設定を適用します。

```html
---
marp: true
theme: uncover
---

<!--
_backgroundColor: black
_color: white
-->

# タイトル ---
<!--
_backgroundColor: orange
paginate: true
-->

# スライド1 テスト ---
<!--
_backgroundColor: white
-->

# スライド2 テスト ---
```

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/a1d40fdb-6da6-134f-1330-69e03cea123c.png)

## 画像をいい感じに扱う - Image syntax

プレゼンには、説明に不可欠なスクリーショットや、目を惹くためにワンポイントで入れたい写真など、画像を使いたいケースがままあります。
背景、レイアウト、画像に対する効果などの文法を紹介します。

```html
---
marp: true
theme: uncover
---

<!--
_color: white
_footer: 'Photo by Benjamin Rascoe on Unsplash'
-->

![bg brightness:0.6](benjamin-rascoe-JS6PY31e2P0-unsplash.jpg) #
タイトル全面背景 Brightnessを落とし、文字の視認性を上げました ---
<!--
_footer: 'Photo by Michal Vasko　on Unsplash'
paginate: true
-->

![bg left:40%](michal-vasko-GOfQNTI_9Og-unsplash.jpg) ### 左に画像をいれる -
表示場所、比率を指定する - 次頁では、複数画像を並べます -
footerで画像クレジット表示も ---
<!--
_backgroundColor: white
_footer: 'Photo by Chris Campbell, Dan on Unsplash'
-->

![bg right:60% contrast:1.5
brightness:1.2](christopher-campbell-rDEOVtE7vOs-unsplash.jpg) ![bg 350%
contrast:1.2 brightness:1](dan-ROJFuWCsfmA-unsplash.jpg) # 進め、 ####
新しいわたし。 なんて、 小洒落た感じにも。 ---
```

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/36749d2c-2f1e-1333-bbde-9115f24af2c8.png)

## カスタムテーマを使う

### カスタムテーマの準備

先ほどの Global Directives で`theme: gaia` を、Local Directives では `theme: uncover` 使いました。

Marp は CSS でテーマを作ることができます。この `theme:` では、あらかじめ同梱されているものを指定できます。デフォルトで `default`, `gaia`, `uncover` 3 つのテーマがあります。

それだけでなく、自作テーマも指定できます。ただ「その自作テーマの存在を VS Code に知らせる」ため、VS Code の `settings.json` に追記が必要です。

VS Code の、 `Code> Preferences> Settings` または、Mac の場合 `Command + , (カンマ)` から、`settings.json`を開きます。次に検索窓へ、`markdown.marp.themes`と入力します。インクリメンタルに要素が検索され、以下の表示にあります。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/e168be64-e911-dc83-32f8-bf1c660aa03f.png)

ここで、`Add Item`ボタンを押し、テーマ CSS ファイルへのパスを入力します。 `./marp-themes/test.css` と入れてみます。今後、複数のテーマを作ったら、追加していくことができます。また、インターネット上のパスを指定することもできます。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/abd7138d-c04d-fe91-125a-b94d1b3f4ebb.png)

### テーマのテンプレート

指定した `./marp-themes/test.css` というファイルを作成します。

```test.css
/* @theme test */

@import 'default';
```

まずこれだけ書きます。 `@import 'default';` により、他に何も指定しないと、一旦デフォルトテーマの CSS が全て継承されています。

次に、`styles_test.md` を作成し、 `theme: test` を指定します。

プレビューはデフォルトテーマのままです。 `theme: test` には、まだデフォルトテーマを継承した内容しか入っていないので、当たり前です。

### テーマをカスタマイズする

さて、ここから実際に CSS に追記し、カスタムテーマを作ってみます。

今回の変更では、ざっくり以下の変更をしていくことにしました。

- タイトルと本体スライドのスタイルを分ける
- タイトル
  - 文字は角ゴシック、白色。サイズ等はよしなに
- 本体
  - 各スライドタイトルは左上寄せ、文字は角ゴシック、青系グラデーション
  - タイトル下に、サブタイトルを同じく左上寄せ、文字は丸ゴシック、青系
  - 本文は、丸ゴシック、黒

以下のように、

- 対象スライドデッキの Markdown ファイル
- カスタムテーマの CSS
- プレビュー

を 3 ペイン開き、Markdown、CSS を編集しながら、リアルタイムにプレビューを確認し、調整していくことができます。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/913fae2a-34de-8478-b007-063dede22a65.png)

最終、このような Markdown, CSS となりました。

```html
---
marp: true
theme: test
---
<!--
class: title
-->

![bg brightness:0.6](benjamin-rascoe-JS6PY31e2P0-unsplash.jpg)

# Custome Themeを適用する

タイトルと、本体スライドのデザインは、classを分けました

---
<!--
class: slides
_footer: 'Photo by Michal Vasko　on Unsplash'
paginate: true
-->

![bg left:40%](michal-vasko-GOfQNTI_9Og-unsplash.jpg)

# グラデーションのタイトル文
## 直下に各スライドのリード文をh2 `##` で入れる。

本文の表示位置は変えていません。

- 箇条書きなどは
- このとおり

---
<!--
_backgroundColor: white
_footer: 'Photo by Chris Campbell, Dan on Unsplash'
-->

![bg right:60% contrast:1.5 brightness:1.2](christopher-campbell-rDEOVtE7vOs-unsplash.jpg)
![bg 350% contrast:1.2 brightness:1](dan-ROJFuWCsfmA-unsplash.jpg)

# 進め、新しいわたし。
## 同様に、h1 `#` でタイトル、</br> h2 `##` でリード文。

自分のテーマが徐々に仕上がってきました。ここからは、皆さんのアレンジ次第!

---
```

```test.css
/* @theme test */

@import 'default';

section {
    font-family: "Arial", "Hiragino Maru Gothic ProN";
    font-size: 32px;
    padding: 40px;
}

section.title h1 {
    color: white;
    font-family: "Arial", "Hiragino Kaku Gothic ProN";
    font-size: 60px;
    text-align: center;
}

section.title {
    color: white;
    font-family: "Arial", "Hiragino Maru Gothic ProN";
    font-size: 36px;
    text-align: center;
    padding: 40px;
}

section.slides h1 {
    background: linear-gradient( to left,  rgba(69,179,224,1) 25%, rgb(97, 109, 218) 75% );
    -webkit-background-clip: text;
    color: transparent;
    font-family: "Arial", "Hiragino Kaku Gothic ProN";
    font-size: 42px;
    position: absolute;
    left: 50px; top: 50px;
}

section.slides h2 {
    font-size: 32px;
    color: #4ac;
    position: absolute;
    left: 50px; top: 90px;
}
```

## 外部ツールの図表を取り込む

スライドの内容に、様々なチャート、グラフをエディタから、または言語やデータセットから生成して表示したいことがあります。Kroki.io, Draw.io, Vega は、そういった時に、すぐに組合わせて使えて便利です。

### Kroki.io

- https://kroki.io/

PlantUML, Graphviz, Mermaid などお馴染みの記法から、少しニッチなものまで、20 以上の記法に対応する、オープンソースのレンダリングエンジン、Web API サービス。

### Draw.io

- https://marketplace.visualstudio.com/items?itemName=hediet.vscode-drawio
- [VSCode で Draw.io が使えるようになったらしい！ - Qiita](https://qiita.com/riku-shiru/items/5ab7c5aecdfea323ec4e)

お馴染みの SVG エディタ。VSCode のプラグインで、オフラインエディタを VSCode 画面内で扱える。またそこで保存した SVG を、そのまま Marp スライド内で活用できる。

### Vega

- https://vega.github.io/vega/
- https://marketplace.visualstudio.com/items?itemName=RandomFractalsInc.vscode-vega-viewer

データセットと、その描画方法を JSON で指定すると、綺麗なグラフを返してくれる Vega。Draw.io 同様、VSCode プラグインがあり、生成した SVG を、Marp スライド内で活用できる。

### 表示例

上の 3 つを、3 スライドに分けて表示してみた例です。それぞれ Kroki は `![bg](GETのendpoint)` 、Draw.io や Vega は `![bg](hoge.svg)` で呼び出しています。

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/64608/19f3b596-c662-3231-0605-8a235e56ffd6.png)

# 関連記事

- 「mmhmm」みたいなプレゼンを、VSCode + Marp で作ったプレゼンに重ねる
  - [【ブラウザだけ、Zoom/ Teams ほかで動く】「mmhmm」のようなプレゼン+人物動画リアルタイム合成を自作してみる - Qiita](https://qiita.com/tomo_makes/items/17dc2732f042238db419)
- スライド内での画像の扱い方
  - 別記事にまとめる予定

# 参考

## 参照したリソース

- 使用したファイル類は、GitHub [tomo-makes/marp-styles](https://github.com/tomo-makes/marp-styles) にまとめました。
- draw.io のファイルを扱うケース、Vega のグラフを取り込む方法について詳しく試行されており、参考にさせていただきました。
  - [Marp for VS Code | Memorandum!](http://ktkr3d.github.io/2020/05/27/Marp-for-VS-Code/)
- サンプルにおける画像素材は、[Unsplash](https://unsplash.com/)の以下作者の皆様の写真を使いました。
  - Benjamin Rascoe 氏
  - Michal Vasko 氏
  - Chris Campbell 氏
  - Dan 氏

## 画像素材、配色について

以下の記事が参考になります。

- [プレゼンテーションに使う画像の探し方 - Qiita](https://qiita.com/TAKAKING22/items/20c006206d2ce23a5608)

# 最後に

- CSS 周りは、普段書いているわけではないので、よりシンプルな書き方、アンチパターンを踏んでいる箇所などあればぜひ、ご指摘お願いします
- 今回は VSCode Preview での出力を中心に取り上げていますが、PDF、HTML などレンダリング時に表示崩れがみられるケースもあります。ご注意ください
- 余談で、この VSCode+Marp から Mac で PDF 出力したものを、Windows 機でフルスクリーン表示しながら Teams 会議でプレゼンすると、画面のちらつきが始って収拾がつかない事象に見舞われたのですが、何かご存知の方、いらっしゃったら教えてください...
