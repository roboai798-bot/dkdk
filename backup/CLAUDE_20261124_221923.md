# CLAUDE.md

## ファイル編集時のバックアップルール（必須）

コードを修正・上書きする前は、**必ず `backup/` フォルダ内に日時付きで変更前のファイルをコピー保存してから編集すること。**

- 対象：既存ファイルを修正・上書き・置換するすべての操作（Edit／Write／スクリプトによる書き換えを含む）
- 保存先：プロジェクト直下の `backup/` フォルダ（無ければ作成する）
- ファイル名：`元のファイル名_YYYYMMDD_HHMMSS.拡張子`
  - 例：`cyberhunter3d.html` → `backup/cyberhunter3d_20261123_193015.html`
- 1回の作業で同じファイルを何度も編集する場合は、最初の編集の直前に1回バックアップを取ればよい
- 新規作成するファイル（既存ファイルが無い場合）はバックアップ不要
- `backup/` 内のファイルは編集・削除しない

### バックアップ手順の例（PowerShell）

```powershell
New-Item -ItemType Directory -Force backup | Out-Null; $ts = Get-Date -Format "yyyyMMdd_HHmmss"; Copy-Item cyberhunter3d.html "backup/cyberhunter3d_$ts.html"
```

### バックアップ手順の例（Bash）

```bash
mkdir -p backup && cp cyberhunter3d.html "backup/cyberhunter3d_$(date +%Y%m%d_%H%M%S).html"
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
