# Claude Certified Architect – Foundations (CCAF) 受験対策リポジトリ

[Claude Certified Architect – Foundations](https://www.anthropic.com/) 認定試験の学習用リポジトリ。
公式の試験ガイド・Practice Exam・Anthropic Academy のコース文字起こし、そして Claude API / Claude Code / MCP の公式ドキュメントのローカルミラーを一箇所に集約しています。

Claude Code で開くと、リポジトリ内の独自 skill (`practice-exam` / `mock-exam`) を使って **Practice Exam の演習** や **オリジナル模試の受験** をそのまま CLI 上で行えます。

---

## 試験範囲 (ドメインと配点)

| # | Domain | Weight |
|---|---|---|
| 1 | Agentic Architecture & Orchestration | 27% |
| 2 | Tool Design & MCP Integration | 18% |
| 3 | Claude Code Configuration & Workflows | 20% |
| 4 | Prompt Engineering & Structured Output | 20% |
| 5 | Context Management & Reliability | 15% |

合格ライン: **720 / 1000** (Practice Exam の推奨目標は 900 / 1000)

詳細は [`Official Exam Guide/Foundations Certification Exam Guide.md`](./Official%20Exam%20Guide/Foundations%20Certification%20Exam%20Guide.md) を参照。

---

## クイックスタート

### 1. Claude Code でこのリポジトリを開く

```bash
cd learn-ccaf
claude
```

[`AGENTS.md`](./AGENTS.md) と [`CLAUDE.md`](./CLAUDE.md) によりプロジェクトの構成情報が自動でコンテキストに読み込まれます。

### 2. Practice Exam を解く (`practice-exam` skill)

Claude に話しかけるだけで起動します:

```
Practice Exam を受けたい
```

→ [`Practice Exam/`](./Practice%20Exam/) 配下の 4 シナリオ x 15 問 = 計 60 問を、**原文ママ**で 1 問ずつ出題。
シナリオ順 / 問題順 / 選択肢順は `shuf` で真にランダム化。回答後に正解と解説を表示し、シナリオ完了ごとに弱点ドメインのサマリを出します。

### 3. オリジナル模試を受ける (`mock-exam` skill)

```
模試を作って
```

→ ドメイン重み (27/18/20/20/15) に従って、ローカルの `docs/` を根拠にした**新規問題**を生成。
問題数 (デフォルト 25) / シナリオ縛り / 言語 / 難易度を最初に確認した後、1 問ずつ出題し、最後に 720/1000 合格ラインで判定 + 弱点ドメインの復習リンクを提示します。

> Practice Exam の問題は流用せず、`docs/` を出典とした検証可能な問題のみを生成します。

---

## ディレクトリ構成

| Path | 内容 |
|---|---|
| [`Official Exam Guide/`](./Official%20Exam%20Guide/) | 公式試験パッケージ (Exam Guide MD/PDF・Welcome・FAQ) |
| [`Practice Exam/`](./Practice%20Exam/) | 4 シナリオ × 15 問の Practice Exam (原文) |
| [`Anthropic Academy/`](./Anthropic%20Academy/) | Anthropic Academy 講座の文字起こしと Course Catalog |
| [`docs/`](./docs/) | Claude API / Claude Code / MCP 公式ドキュメントのローカルミラー |
| [`.claude/skills/`](./.claude/skills/) | `practice-exam` / `mock-exam` skill |

詳細なファイル単位の説明は [`AGENTS.md`](./AGENTS.md) を参照してください。

---

## 学習の進め方 (推奨フロー)

1. **試験範囲を把握する** — [`Official Exam Guide/Foundations Certification Exam Guide.md`](./Official%20Exam%20Guide/Foundations%20Certification%20Exam%20Guide.md) のドメインと task statement を一通り読む
2. **講座で土台を作る** — [`Anthropic Academy/`](./Anthropic%20Academy/) の 4 講座 (Building with the Claude API / Claude Code in Action / MCP 入門 / MCP 上級) を流し読み
3. **公式ドキュメントを参照する** — [`docs/`](./docs/) のミラーを使って詳細を確認 (ネット接続なしでも grep 可能)
4. **Practice Exam を解く** — `practice-exam` skill を呼び出して 4 シナリオを順に消化、弱点ドメインを把握
5. **模試で仕上げる** — `mock-exam` skill で弱点ドメイン中心の模試を作成、合格ライン (72%) を安定して超えるまで反復

---

## Skill の中身を見る・カスタマイズする

| Skill | 定義ファイル |
|---|---|
| `practice-exam` | [`.claude/skills/practice-exam/SKILL.md`](./.claude/skills/practice-exam/SKILL.md) |
| `mock-exam` | [`.claude/skills/mock-exam/SKILL.md`](./.claude/skills/mock-exam/SKILL.md) |

問題数のデフォルト・シナリオ縛り・言語切替などはこの 2 ファイルを編集すれば挙動を変えられます。
