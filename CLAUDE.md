# CLAUDE.md

## ファイル編集時のバックアップルール（必須）

ゲームHTMLファイルを修正・上書きする前は、**必ずタイトルごとのバックアップフォルダにコピーを保存し、バックアップ作成が完了してから本体を修正すること。**

### 対象
- このフォルダ内の既存ゲームHTMLファイル（`bomberman3d.html`、`cyberhunter3d.html`、`ironape3d.html`、`okuscroll3d.html` など）
- **今後このフォルダに新規作成されるすべてのゲームHTMLファイル**（作成後に修正・上書きする時点から本ルールを適用する）
- 修正・上書き・置換するすべての操作（Edit／Write／スクリプトによる書き換えを含む）
- ゲーム以外の既存ファイル（`index.html`、`CLAUDE.md` など）を指示により修正する場合も同じ形式でバックアップを取る

### 保存先とファイル名
- 保存先：`backup/<拡張子を除いたファイル名>/`（タイトルごとにフォルダを分けて管理）
  - 例：`bomberman3d.html` → `backup/bomberman3d/`、新作 `新作名.html` → `backup/新作名/`
- フォルダが存在しない場合は自動で作成する
- ファイル名：`元のファイル名_YYYYMMDD_HHMM.拡張子`
  - 例：`bomberman3d.html` → `backup/bomberman3d/bomberman3d_20261124_2215.html`
- 同じ分に複数回バックアップする場合は上書きせず、末尾に `_2` などを付けて別名で保存する
- 1回の作業で同じファイルを何度も編集する場合は、最初の編集の直前に1回バックアップを取ればよい
- 新規作成するファイル（既存ファイルが無い場合）は作成時のバックアップ不要
- `backup/` 内のファイルは編集・削除しない（過去に `backup/` 直下へ保存したファイルもそのまま残す）

### 編集対象の制限
- 修正対象として明示されたファイル以外の既存HTMLファイルや `index.html` は、明示的な指示がない限り**絶対に編集・上書き・削除しない**

### バックアップ手順の例（PowerShell）

```powershell
$f = "bomberman3d.html"; $n = [IO.Path]::GetFileNameWithoutExtension($f); New-Item -ItemType Directory -Force "backup/$n" | Out-Null; $ts = Get-Date -Format "yyyyMMdd_HHmm"; Copy-Item $f "backup/$n/${n}_$ts.html"
```

### バックアップ手順の例（Bash）

```bash
f=bomberman3d.html; n="${f%.*}"; mkdir -p "backup/$n" && cp "$f" "backup/$n/${n}_$(date +%Y%m%d_%H%M).html"
```

## 修正後の GitHub への commit / push（必須）

コードやファイルを修正したら、作業の区切りごとに **必ず git で commit し、GitHub（`origin` = https://github.com/roboai798-bot/dkdk.git の `main`）へ push すること。**

- タイミング：ユーザーから依頼された修正が完了し、動作確認まで終わった時点で毎回行う（確認を求めずに実行してよい）
- 対象：変更したファイルと、バックアップルールで作成した `backup/` 内のファイルも含めてコミットする
- コミットメッセージ：何を変えたかが分かる日本語または英語の短い要約にする
- 除外：`.gitignore` の対象（`.claude/settings.local.json` など）や、秘密情報を含むファイルはコミットしない
- push に失敗した場合（認証切れ・リモートとの競合など）は、強制 push（`--force`）はせず、原因をユーザーに報告する

### 手順の例

```bash
git add -A && git commit -m "変更内容の要約" && git push origin main
```
