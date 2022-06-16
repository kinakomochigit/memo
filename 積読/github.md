# GitHub で issue を登録する時に自動でプロジェクトを指定する

GitHub で issue を登録する時に毎度プロジェクト指定を手動で行うのが面倒だったので自動で設定する方法をまとめました。ググってみると GitHub Actions を使った方法が多かったのですが、 この方法では GitHub Actions を利用しません。

![スクリーンショット 2021-09-03 9.29.34.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/19698/885e785b-a7ab-26e7-74cd-d9fff6ef66c7.png)
↑やりたいこと


## テンプレート機能でできない？

GitHub には Issue や Pull Request の作成時に使える[テンプレート機能](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/about-issue-and-pull-request-templates)があります。

assignees や labels は以下のようにテンプレートで指定できるので projects も指定できるようになってほしいところですが、この記事の執筆時点ではそのような仕様はないようです。こちらが対応されたらこの記事の方法は採らなくて良いと思います。

```md:.github/ISSUE_TEMPLATE/issue.md
---
name: 🐞 Bug
about: File a bug/issue
title: '[BUG] <title>'
labels: Bug, Needs Triage
assignees: ''
---

## TODO
- [ ] 
```

## 方法

GitHub の issue 登録時の URL には [クエリパラメータ](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue#creating-an-issue-from-a-url-query) を設定できますが、これで projects を指定できます。これと合わせて、テンプレート選択画面をカスタマイズする `config.yml` を用いればプロジェクト指定が可能になります。`config.yml` では `contact_links` の部分に遷移先 URL を指定できるので、 Issue 作成　URL のクエリパラメータに projects を指定します。以下の値はそれぞれの環境に置き換えてください。

- `{ORGANIZATION OR REPO}` : GitHub Project が存在する Organization もしくはリポジトリ
- `{REPO}` : Issue を作成するリポジトリ
- `{PROJECT_NUMBER}` : GitHub Project の自動で採番された数字

```yaml:.github/ISSUE_TEMPLATE/config.yml
blank_issues_enabled: true
contact_links:
  - name: Issue
    about: Issue template
    url: https://github.com/{REPO}/issues/new?template=issue.md&projects={ORGANIZATION OR REPO}/{PROJECT_NUMBER}&labels=enhancement
```

例えば [tanabee/issue-automation-sample](https://github.com/tanabee/issue-automation-sample) リポジトリで [tanabee/issue-automation-sample/projects/1](https://github.com/tanabee/issue-automation-sample/projects/1) の GitHub Project を指定する場合は url 部に `https://github.com/tanabee/issue-automation-sample/issues/new?template=issue.md&projects=tanabee/issue-automation-sample/1&labels=enhancement` と記述します。

Organization 直下のプロジェクトと紐付ける場合は `{ORGANIZATION OR REPO}` 部分に Organization 名を挿入します。

クエリパラメータで `template=issue.md` を指定していますが、 issue.md 内に assignees や labels の記述があっても解釈してくれないようです。そのため、 assignees や labels を指定したい場合は config.yml のクエリパラメータで指定する必要があります（指定できるパラメータは [Creating an issue from a URL query](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue#creating-an-issue-from-a-url-query) を参照）。特にテンプレートの本文が不要な場合は `template=issue.md` の指定を除いても問題ありません。

最終的には以下の2つのファイルになりました。

```yaml:.github/ISSUE_TEMPLATE/config.yml
blank_issues_enabled: true
contact_links:
  - name: Issue
    about: Issue template
    url: https://github.com/{REPO}/issues/new?template=issue.md&projects={ORGANIZATION OR REPO}/{PROJECT_NUMBER}&labels=enhancement
```

[.github/ISSUE_TEMPLATE/config.yml](https://github.com/tanabee/issue-automation-sample/blob/main/.github/ISSUE_TEMPLATE/config.yml)

```md:.github/ISSUE_TEMPLATE/issue.md
## TODO
- [ ] 
```

[.github/ISSUE_TEMPLATE/issue.md](https://github.com/tanabee/issue-automation-sample/blob/main/.github/ISSUE_TEMPLATE/issue.md)

実際に動作するリポジトリを用意したので、分からない場合にはご参考ください。

- リポジトリ: https://github.com/tanabee/issue-automation-sample
- GitHub Project: https://github.com/tanabee/issue-automation-sample/projects/1

以上。

[GitHub で issue を登録する時に自動でプロジェクトを指定する](https://qiita.com/tanabee/items/bb8817cf683fc35f0b05)