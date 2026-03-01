# CLAUDE.md — learn_claude プロジェクト

## このプロジェクトについて

Claude Code および OpenAI Codex CLI を学ぶためのカリキュラム管理リポジトリ。
各ツール・OS向けのカリキュラムファイルを継続的に改善・拡充する。

## 設計方針（変えてはいけないこと）

- **フェーズ分離**: 各フェーズは独立したClaude Codeセッションで完結させる（コンテキスト管理のため）
- **フェーズ末コミット**: 各フェーズ末に必ず git commit & push
- **必須習得項目**: Skills / Subagent / Agent Teams / Plan Mode / コミュニケーション保存
- **ライセンス安全**: Python環境ツールは企業でも使えるもののみ（uv, venv, pip）
- **クリーンマシン想定**: 何もインストールされていない状態からスタートする前提
- **GitHub Flow**: feature branch → commit → push → PR → merge
- **段階的自動化**: まず手動で仕組みを理解し、その後Skillsで自動化する体験順序を守る

## ファイル構成

| ファイル | 内容 |
|---------|------|
| `CURRICULUM_CLAUDE_MAC.md` | Claude Code macOS版カリキュラム（Phase 0〜9） |
| `CURRICULUM_CLAUDE_WINDOWS.md` | Claude Code Windows版カリキュラム（Phase 0〜9） |
| `CURRICULUM_CODEX_MAC.md` | Codex CLI macOS版カリキュラム（Phase 0〜9） |
| `CURRICULUM_CODEX_WINDOWS.md` | Codex CLI Windows版カリキュラム（WSL2前提）（Phase 0〜9） |
| `CLAUDE.md` | このファイル。設計方針・状態・変更履歴を記録する |

## 現在の状態

- `CURRICULUM_CLAUDE_MAC.md`: Phase 0〜9 完成（旧 CURRICULUM.md）
- `CURRICULUM_CLAUDE_WINDOWS.md`: Phase 0〜9 完成（旧 CURRICULUM_WINDOWS.md）
- `CURRICULUM_CODEX_MAC.md`: Phase 0〜9 完成
- `CURRICULUM_CODEX_WINDOWS.md`: Phase 0〜9 完成（WSL2前提）
- `/end-phase` スキル: Phase 3 タスクP3-6として組み込み済み
- `/rename` `/tag` の説明: Phase 1 タスクP1-5に追加済み
- `Skills/カスタムコマンド`の説明: Phase 2に3種の区別（自動起動Skills・カスタムコマンド・組み込みコマンド）を追記済み
- `/revise-claude-md` プラグイン: Phase 0 P0-5のsettings.json設定に有効化手順を追記済み

## カリキュラム改善時のワークフロー

```
1. GitHubでIssueを作成（「なぜ変えるか」を記録しておく）
2. feature/xxx ブランチを作成
3. Claude Codeで編集（このCLAUDE.mdを読んで文脈を把握してから着手）
4. コミット時にIssue番号を含める（例: "feat: add phase X detail (#5)"）
5. PRを作成 → mainにマージ
```

## 変更履歴

| 日付 | 変更内容 |
|------|---------|
| 2026-02-27 | CURRICULUM.md 初版作成（Phase 0〜9、macOS版） |
| 2026-02-27 | CURRICULUM_WINDOWS.md 作成（Windows版） |
| 2026-02-27 | `/rename` `/tag` の意味をPhase 1 P1-5に追加；`/end-phase` スキルをPhase 3 P3-6として追加 |
| 2026-02-27 | CLAUDE.md 作成（コンテキスト保全のため） |
| 2026-02-28 | CURRICULUM.md → CURRICULUM_CLAUDE_MAC.md、CURRICULUM_WINDOWS.md → CURRICULUM_CLAUDE_WINDOWS.md にリネーム |
| 2026-02-28 | CURRICULUM_CODEX_MAC.md・CURRICULUM_CODEX_WINDOWS.md 新規作成（Codex CLI版） |
| 2026-03-02 | Phase 0 P0-5にclaude-md-managementプラグイン有効化手順を追加；Phase 2「Skillsとは」説明を修正（自動起動Skills・カスタムコマンド・組み込みコマンドの区別を明記）(#1) |

## 次に改善したいこと（アイデアメモ）

（アイデアが浮かんだらここに箇条書きで追記。GitHubのIssueに移行したら削除する）
