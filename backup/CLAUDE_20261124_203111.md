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
