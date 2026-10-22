# 開発ルール

## ブランチの運用
- GitHub Flow（main ＋ 作業ブランチ）。develop は作らない
- main は保護されていて、直接 push できない。変更は必ず PR を通す

## ブランチ名/コミットメッセージ
| ブランチ名 | コミットメッセージ | 使う場面 | 
| -- | -- | -- |
| `feat/` | `feat:` | 新しい機能などの追加 |
| `fix/` | `fix:` | プログラムのバグや不具合、意図しない挙動の修正 |
| `docs/` | `docs:` | ドキュメントの変更や追加 |
| `ci/` | `ci:` | GitHub ActionsやCircleCIなどのCI/CD設定ファイルの変更 |
| `chore/` | `chore:` | その他の雑多な作業 |
| `test/` | `test:` | testコードの修正や追加　| 

コミットメッセージはプレフィクスは英語、説明は日本語にする。

例: `feat: Cargoプロジェクトの雛形とVM仕様を追加`

## 作業の流れ
1. main を最新にする
   ```
   git switch main
   git pull
   ```
2. 作業ブランチを作る
   ```
   git switch -c ブランチ名
   ```
3. 実装 -> `cargo test` -> コミット
   ```
   cargo test
   (cargo test it_works / cargo test テスト名)
   git add 対象ファイル
   git commit -m "prefix: メッセージ"
   ```
4. push する（初回だけ `-u` を付ける）
   ```
   git push -u　origin ブランチ名
   git push # 2回目以降
   ```
5. GitHubでPRを作る（作業branch -> main）
6. 差分を見直して Squash merge する
7. 後片付けをする
   ```
   git switch main
   git pull
   git fetch --prune
   git branch -D ブランチ名
   ```


## PR を出す前に確認すること
- [ ] `cargo test` が通る
- [ ] 差分を自分で見直した
- [ ] 仕様を変えたときはREADMEの命令セット表も更新した
<!-- TODO: CI を入れたら fmt / clippy を追加 --> 

## マージとその後
- Squash merge だけを使う
- マージ後は `git branch -D` で手元のブランチを消す
  - GitHub でPRがMergedになっていることを確認してから使う
  - squashだと、複数の変更が1つのコミットにまとめられてmainに入れられる。
  - GitはコミットをIDで区別しているため、まとめられたコミットと複数の変更の中身が同じ場合でも、IDが異なる新しいコミットとしてmergeするため、Gitから見ると、mainに入っていない変更がブランチに残っている様に見えてしまう。
  - そのため、普通の削除 `-d`だと「まだmergeされていません」と止めてくれる。そのため、強制的に削除するために `-D`で消して、不要なブランチは削除する。
