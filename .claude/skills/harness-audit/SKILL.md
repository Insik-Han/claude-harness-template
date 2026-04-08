---
name: harness-audit
description: "Claude Code ハーネス構成（CLAUDE.md, settings.json, hooks, agents, skills, rules）をベストプラクティスに照らして監査する。ハーネスの追加・変更後、定期メンテナンス時、または構成の品質を確認したいときに使用。entropy-detector がコード健全性を見るのに対し、こちらはハーネス構成自体の品質を検査する。"
user-invocable: true
argument-hint: "[--focus area] — area: claude-md|hooks|agents|skills|rules|all"
---

# Harness Audit スキル

Claude Code のハーネス構成（`.claude/` ディレクトリ + `CLAUDE.md`）をベストプラクティスに照らして監査し、改善提案を出す。

## 使い方

```
/harness-audit              # 全領域を監査
/harness-audit --focus hooks   # hooks のみ重点監査
```

## 監査手順

### 0. ファイル収集

以下のファイルを Read / Glob で収集する（存在しないものはスキップ）:

- `CLAUDE.md`
- `.claude/settings.json`
- `.claude/settings.local.json`（あれば）
- `.claude/agents/*.md`
- `.claude/skills/*/SKILL.md`
- `.claude/rules/*.md`

---

### 1. CLAUDE.md 品質

| チェック項目 | 基準 | 根拠 |
|---|---|---|
| 行数 | ~100行（200行以下） | CLAUDE.md はマップ、百科事典ではない |
| 技術スタック | package.json の主要依存と一致 | エージェントが古い技術前提で作業するリスク |
| コマンド一覧 | package.json scripts と一致 | 存在しないコマンドの実行を防ぐ |
| スキルテーブル | テーブル掲載スキルが `.claude/skills/` に実在する | 削除済みスキル参照の検出 |
| エージェントテーブル | テーブル掲載エージェントが `.claude/agents/` に実在する | 削除済みエージェント参照を検出 |
| Hooks セクション | CLAUDE.md 記載の hooks が `.claude/settings.json` に存在する | 主要フックのみ整合を求める |
| リンク健全性 | 記載された全パスが実在する | 壊れたリンクはエージェントを迷わせる |

---

### 2. settings.json — Hooks

| チェック項目 | 基準 | 根拠 |
|---|---|---|
| PreToolUse matcher | 各 hook に `matcher` が設定されている | matcher なしは全ツールに発火し過負荷 |
| 危険操作ガード | `rm -rf`, `DROP TABLE`, `--force`, `--no-verify` をブロック | 不可逆操作の防止 |
| SessionStart | ブランチ・変更状態の表示 | セッション開始時のコンテキスト把握 |

---

### 3. Agents

各 `.claude/agents/*.md` について:

| チェック項目 | 基準 | 根拠 |
|---|---|---|
| frontmatter 必須フィールド | `name`, `description` が存在する | 自動検出・呼び出しに必須 |
| description の質 | WHEN（いつ使うか）を含む | トリガー精度に直結 |
| model 指定 | 用途に応じた model が明示されている | コスト最適化 |
| 参照パスの実在 | agent 内で参照するファイルパスが全て実在する | 壊れた参照は空振り |
| examples | description に具体的な使用例がある | auto-invoke の精度向上 |

---

### 4. Skills

各 `.claude/skills/*/SKILL.md` について:

| チェック項目 | 基準 | 根拠 |
|---|---|---|
| frontmatter 必須フィールド | `name`, `description`, `user-invocable` が存在する | 検出・メニュー表示に必須 |
| description の質 | いつ使うか + 何をするかの両方を含む | under-trigger 防止 |
| 行数 | SKILL.md が 500行以下 | 超える場合は references/ に分割 |

---

### 5. Rules

| チェック項目 | 基準 | 根拠 |
|---|---|---|
| カバレッジ | testing, security, git-workflow は最低限存在 | 基本的な品質ゲート |
| 相互参照 | development-workflow.md が参照するスキル・エージェントが実在 | フロー定義の整合性 |
| 重複 | rules と CLAUDE.md で同じ内容を二重管理していない | 更新漏れの原因 |

---

## 出力フォーマット

### Findings

| Severity | 基準 |
|---|---|
| **CRITICAL** | エージェントの動作に直接影響（壊れた参照、不一致） |
| **HIGH** | ベストプラクティスからの重大な逸脱 |
| **MEDIUM** | 改善推奨だが動作には影響しない |
| **LOW** | 細かい品質向上の提案 |

### Health Score

各領域を 5 段階で評価。

### Recommendations

優先度順に最大 5 件の具体的アクションを提案する。

---

## ガイドライン

- 実ファイルを Read して検証する — 推測しない
- 本質的な問題のみ報告する — スタイルの好みは報告しない
- 各 finding に具体的な修正方法を含める
- 問題がなければ「健全」と報告する — 問題を捏造しない
