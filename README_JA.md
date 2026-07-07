# Cockpit Best Practices

AGI Cockpit のタスク運用判断を安全にするための Codex skill です。

この skill はコマンドリファレンスではなく、運用ポリシーに特化しています。
コマンドの構文や実行は `cockpit` skill を使い、タスクを完了するべきか、
削除するべきか、子タスクや worktree をどう扱うべきかの判断に
`cockpit-best-practices` を使います。

## 対象範囲

- `running`、`waiting_confirmation`、`completed`、`needsResume`、pinned task の扱い
- complete と remove の使い分け
- 親タスクと子タスクの委任ルール
- worktree 付きタスクを閉じる前の確認
- autorun タスクの衛生管理
- `cockpit-gc` の候補をどう解釈するか
- 破壊的な cleanup を避けるための安全ルール

## インストール

このディレクトリを Codex の skills ディレクトリにコピーします。

```sh
mkdir -p ~/.codex/skills
cp -R cockpit-best-practices ~/.codex/skills/
```

新しい Codex セッションで次のように呼び出します。

```text
$cockpit-best-practices
```

## 使い方

基本的には `cockpit` skill と組み合わせます。

```text
$cockpit $cockpit-best-practices
waiting の Cockpit タスクを確認し、完了してよいものを提案してください。
```

backlog cleanup では `cockpit-gc` と組み合わせます。

```text
$cockpit-gc $cockpit-best-practices
cleanup 候補を探してください。ただし承認なしに complete/remove しないでください。
```

## 安全方針

この skill は、有用なタスク履歴を残すことを優先します。タスクを閉じる前に、
重要な知識を project files、commits、docs、issues、PR などへ移すことを推奨します。

また、タスクの complete/remove、process kill、worktree deletion、autorun 変更、
未確認の子タスク出力の破棄は、明示的なユーザー承認が必要な操作として扱います。

## 構成

- `SKILL.md` - Codex skill 本体
- `agents/openai.yaml` - AGI Cockpit/OpenAI 用の表示メタデータ
- `README.md` - 英語版 README

