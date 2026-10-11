# 同期と引継ぎ — 初心者向け

## 1. 最初の考え方

GitHubは公開コードの履歴、Driveは内部の作業交接です。
既存の交接台を引き続き使い、同じ名前の新しい台帳は作りません。
交接台のURLはブラウザーのブックマークに保存し、公開READMEへ貼りません。
古い「オンライン／オフライン」表記は、日時を付けて再確認するまで現在の状態と扱いません。

commit（提交）は変更を履歴に保存、push（推送）はGitHubへ送信、
pull（拉取）はGitHubから取得・統合、branch（分支）は変更を分ける作業場所です。
公開リポジトリのbranchも公開です。

## 2. Windowsで初回導入：画面操作がおすすめ

1. 既存のPCプロジェクトを別の非公開フォルダーにコピーして保存する。
2. [GitHub Desktop](https://desktop.github.com/)をインストールし、自分のGitHubアカウントでログインする。
3. File → Clone repository（仓库克隆）→ URLで https://github.com/AOI-101/aoi を指定する。
4. Local pathは既存プロジェクトとは別の新規フォルダーにする。Drive同期フォルダーの中には置かない。
5. Current branch → New branchで pc/initial-import などの作業ブランチを作る。
6. 既存PCの公開可能なファイルだけを、比較しながら新しい作業フォルダーへ取り込む。
   .gitや認証情報、内部文書、依存パッケージを丸ごとコピーしない。
7. Changesで全差分を読む。必要なファイルだけチェックし、Summaryに理由を書いてCommitする。
8. Publish branch／Push originで送信し、GitHub上でPull request（合并请求）を作る。
9. Files changedで内容を確認し、必要な実行確認の後にMergeする。
10. PCではmainに切り替えてFetch origin → Pull originで更新する。

PCに既存のGit履歴がある場合は、そのフォルダーのremote設定を変更せず、
新しいcloneと別に保存したうえでCodexに履歴・差分の比較を依頼する。
以降どちらを作業用フォルダーにするか決めるまでは、旧版を削除しない。

## 3. スマートフォンで作業する

内部メモだけなら、Google Docsアプリで既存交接台の「Mobile 工作区」へ追記する。
PC欄を上書きせず、日時、元の版、追加内容、次の一手を書く。
ファイルは別名でDriveに保存する。GitやSSHの導入は不要。

公開コードを小さく修正する場合：
1. ブラウザーでこのリポジトリを開く。操作が見えない場合はPC版表示を使う。
2. mainを元に mobile/日付-短い作業名 の新しいbranchを作り、そのbranchを選ぶ。
3. 対象ファイルの編集、またはAdd file → Create new file／Upload filesで変更する。
4. 内容が公開可能か確認してCommit changesする。
5. Compare & pull requestでmain向けのPRを作り、PCで確認できるまでmergeを待つ。
6. Drive側にbranch・PRと未検証事項を記録する。

GitHubモバイルアプリに同じ編集機能が見つからない場合はブラウザーを使う。
秘密の作業は公開branchに下書きしない。

## 4. 離席前・帰宅後

離席前：PC変更を安全に保存し、Driveへ元の版と未同期変更、次の一手を書く。
公開可能で確認済みの作業だけをGitHubへpushする。
端末が電源OFFでもDrive上のメモは使えるが、PC内部の未送信ファイルは読めない。

帰宅後：
1. PC現行版とMobile成果物を別々に非公開で保存する。
2. PCの未コミット変更を確認する。未保存のままpullやbranch切替を進めない。
3. GitHubの変更をfetch（取得だけ）し、PC・GitHub・Drive追加内容を比較する。
4. 競合があれば、どの部分を採用するか確認して作業branchに統合する。
5. 実行・内容を確認してPRをmergeし、PCのmainを更新する。
6. Driveの「合并记录」へ採用・保留・結果・版を残す。

PCとMobileで同じファイルを変更していた場合、「新しい日時だから正しい」と決めない。
Codexへ依頼する文：
「PCの未同期変更とモバイル成果物を別々に保存し、差分を一覧にしてください。
競合は確認するまで上書きせず、公開可能な変更だけを作業ブランチへ統合してください。」

## 5. PowerShellを使う場合（任意）

[Git for Windows](https://git-scm.com/downloads/win)導入後、新しい作業場所で実行する。
以下は初回cloneの例。既にclone済みなら繰り返さず、そのフォルダーを開く。

~~~powershell
git clone https://github.com/AOI-101/aoi.git aoi-work
cd aoi-work
git status
git switch -c pc/first-change
~~~

必要なファイルを編集した後、例としてREADMEだけを送る場合：

~~~powershell
git diff -- README.md
git add -- README.md
git diff --cached
git commit -m "docs: clarify setup"
git push -u origin pc/first-change
~~~

作者名・メールの設定を求められたら、自分が公開してよい名前と、
GitHub設定画面で確認したnoreplyメールをこのリポジトリ用に設定する。
値を推測しない。認証は正規のログイン画面を使い、トークンをコマンドやチャットへ貼らない。

PRをGitHubでmergeした後、作業ツリーがcleanであることを確認して更新：

~~~powershell
git status
git switch main
git pull --ff-only
~~~

pullが拒否された場合は止めてgit statusと差分を確認する。
force push、reset --hard、git cleanを解決方法として使わない。
.gitignoreで除外された内部ファイルはcommitされないため、非公開の別コピーで保護する。

## 6. Codexの利用範囲

GitHub接続がある場合は対象リポジトリとbranchを指定する。
接続がない場合は公開可能なファイルだけを渡し、変更済みファイルや差分を受け取る。
Drive参照権限、GitHub書込権限、PC操作権限はそれぞれ別に確認する。
この初期化は自動同期、リモートデスクトップ、SSH、常時接続を設定しない。

参考：[GitHub公式 入門](https://docs.github.com/en/get-started/start-your-journey)
