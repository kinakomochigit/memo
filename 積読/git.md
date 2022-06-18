# configファイルの場所  
## local  
[リポジトリのルートディレクトリ]¥.git¥config

## global  
C:¥Users¥[ユーザー名]¥.gitconfig

## system  
[gitインストール先]¥etc¥gitconfig  
[gitインストール先]¥mingw64¥etc¥gitconfig  
一般的な場所  
C:¥Program Files¥Git¥etc¥gitconfig  
C:¥Program Files¥Git¥mingw64¥etc¥gitconfig  
C:¥Program Files (x86)¥Git¥etc¥gitconfig  
C:¥Program Files (x86)¥Git¥mingw64¥etc¥gitconfig  

[[Git]configファイルの場所(macOS / Windows)](https://www.curict.com/item/fe/fe45741.html)

# Git 設定の 3 つのスコープ
git config による設定のスコープは 3 種類あり、スコープが狭くなるほど参照時の優先度は高くなります。 下記はそれぞれのスコープでの設定方法を、優先度の高い順に示しています。 カッコの中のファイル名は、コマンドを実行したときの設定値の保存先です。

```
git config --local ...   # 各リポジトリごとの設定 (.git/config)（優先度:高）
git config --global ...  # 現在のユーザの共通設定 (~/.gitconfig)
git config --system ...  # システム内の共通設定 (/etc/gitconfig など)（優先度:低）
```

例えば、global 設定で user.name が Ichiro になっていても、local 設定が Jiro になっていれば、Jiro の方が優先的に使用されます。

プロジェクトごとに固有の設定をする場合は、local なスコープで設定を行うとよいでしょう。 この場合、プロジェクトの作業ツリーのトップにある .git/config に設定が保存されます。

[Git の local 設定と global 設定と system 設定の違い](https://maku77.github.io/git/settings/local-global-system.html)

# 複数のgitアカウントを使い分ける
## globalにアカウント情報を設定する

```
$ git config --global user.name "メインアカウント"
$ git config --global user.email "メインアカウントメールアドレス"
```

### 反映しているか確認

```
$ cat ~/.gitconfig

[user]
	name = メインアカウント
	email = メインアカウントメールアドレス
```




## localにアカウント情報を設定する

※リポジトリ内の`./.git/config`に設定する。

```
cd Project/develop
$ git config --local user.name "サブアカウント"
$ git config --local user.email "サブアカウントメールアドレス"
```

### ./.git/configへ設定

```
$ cat ~/.gitconfig

[user]
	name = サブアカウント
	email = サブアカウントメールアドレス
```


## <i class="fa fa-refresh" aria-hidden="true"></i> アカウント反映がされているかの確認

メインアカウントとサブアカウントでそれぞれ、`git commit`した後に、`git log`で反映されているか、確認する。

```
$ git log

commit XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Author: サブアカウント <サブアカウントメールアドレス>
Date:   Xxx Xxx 00 00:00:00 2016 +0000

    add:git アカウント設定
```
[複数のgitアカウントを使い分ける](https://qiita.com/0084ken/items/f4a8b0fbff135a987fea)

# git pull を強制し、リモートでローカルを上書きする方法
ローカルのmasterを、強制的にリモートのmasterに合わせる

```
// 1) リモートの最新を取ってきておいて・・
$ git fetch origin master

// 2) ローカルのmasterを、リモート追跡のmasterに強制的に合わせる！
$ git reset --hard origin/master
```

「git pull の強制」というよりは、要は「reset」という方が正しいですね。

もちろん、git reset --hardは、手元にある作業ツリーとインデックスの変更内容は、すべてふっとんで消えてなくなりますので、実行前は注意して慎重に行って下さい。

一般的にgitでは、「コミットされていない変更」は、一度失うともう帰ってこないですので、不安な人は必ず実行前に、git statusして、作業ツリーの状態を確認して下さい。もし、作業ツリーとインデックスを別の場所に退避しておきたかったら「git stash」などがあります。

[ローカルのmasterを、強制的にリモートのmasterに合わせる](https://www-creators.com/archives/1097)