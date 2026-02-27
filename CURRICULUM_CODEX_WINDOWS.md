# Codex CLI バイブコーディング学習カリキュラム（Windows / WSL2版）

## このドキュメントの使い方

このファイル自体がカリキュラムです。上から順番に読み進め、各タスクのプロンプトをCodex CLIに投げながら学習を進めてください。

**学習スタイル:** バイブコーディング（AIと対話しながらコードを作り上げる）
**制作物:** Python CLIタスク管理ツール「taskr」
**期間目安:** 3週間（週5〜10時間）
**動作環境:** Windows 10 / 11 + WSL2（Ubuntu 22.04）

> **重要:** Codex CLIのWindowsネイティブ対応は実験的（Experimental）のため、このカリキュラムでは**WSL2上のUbuntu 22.04**での実行を推奨します。Phase 0冒頭でWSL2のセットアップを実施します。

---

## 変更履歴

| 日付 | 変更内容 |
|------|---------|
| 2026-02-28 | 初版作成（Phase 0〜9、Codex CLI Windows / WSL2版） |

---

## Codex CLI とは（Claude Codeとの違い）

| 機能 | Claude Code | Codex CLI |
|------|-------------|-----------|
| インストール | `npm i -g @anthropic-ai/claude-code` | `npm i -g @openai/codex` |
| プロジェクト設定ファイル | `CLAUDE.md` | `AGENTS.md` |
| グローバル設定 | `~/.claude/settings.json` | `~/.codex/config.toml` |
| Plan Mode | Shift+Tab×2で起動 | **なし**（`/status` + Suggest Modeで代替） |
| 承認モード | スマートサンドボックス | Suggest / Auto-Edit / Full-Auto の3段階 |
| セッション管理 | 暗黙的（CLAUDE.md） | 明示的（`codex resume --last` / `/resume`） |
| カスタムコマンド | Skillsフォルダ | `~/.codex/` 内の`.md`ファイル（ファイル名=コマンド名） |
| MCP設定 | `.mcp.json` / `settings.json` | `config.toml` の `[mcp_servers.<name>]` |
| CI/CDスクリプト | 非対応 | `codex exec` コマンド |
| Windows対応 | ネイティブ対応 | **WSL2必須**（ネイティブは実験的） |
| 通知フック | macOS: osascript | WSL2: `echo` / `notify-send` |

---

## ⚠️ コンテキスト管理ルール（重要）

**各フェーズは独立したCodex CLIセッションで実施すること。**

フェーズ完了の手順:
1. フェーズ内のタスクをすべて完了
2. featureブランチを commit → push → Pull Request作成（フェーズにより手動/カスタムコマンド）
3. `/fork` で現在のセッションを分岐・保存し、AGENTS.mdに次フェーズの引き継ぎ情報を記録する
4. Codex CLI セッションを終了（`/quit` または `Ctrl+D`）
5. 次のフェーズは **新しいセッション** で開始（Codex CLIはAGENTS.mdを読んでコンテキストを把握する）

**なぜセッションを分けるか:**
- コンテキストウィンドウの使用量を管理するため
- フェーズの境界を明確にするため
- AGENTS.mdが「セッション間の引き継ぎノート」として機能するため

---

## GitHub Flow の学習ステップ（自動化の段階的移行）

| フェーズ | GitHub Flow | 操作 |
|---------|------------|------|
| Phase 1 | 手動フルフロー | `git checkout -b` → `git add` → `git commit` → `git push` → `gh pr create` |
| Phase 2〜3 | カスタムコマンド初体験 | 手動branch + **`/commit`**（メッセージ自動生成）+ 手動push/PR |
| Phase 4〜5 | /commit-push-pr | 手動branch + **`/commit-push-pr`**（add+commit+push+PR一括） |
| Phase 6〜 | 完全自動 | プロンプト一発でbranch〜PRまで全自動 |
| Phase 7〜9 | Subagent統合 | 実装完了後に自動PR・自動マージ |

---

## フェーズ一覧

| フェーズ | 内容 | 主な習得機能 | 目安時間 |
|---------|------|------------|---------|
| **事前準備** | WSL2 + VS Code + Codex CLI インストール | - | 1時間 |
| **Phase 0** | WSL2環境構築 + Python環境（Codex CLI半自動） | uv, gh CLI, GitHub連携, AGENTS.md | 1〜2時間 |
| **Phase 1** | 基本対話・Suggest Mode・コミュニケーション保存 | /status, AGENTS.md, セッション管理 | 2時間 |
| **Phase 2** | 初期実装 + git統合 + カスタムコマンド | /commit, バイブコーディング | 3時間 |
| **Phase 3** | Hooks + カスタムコマンド作成 | notify設定, カスタムコマンド自作 | 2〜3時間 |
| **Phase 4** | MCP統合 + Subagent | GitHub MCP, Subagent委託 | 2〜3時間 |
| **Phase 5** | 承認モード（Suggest/Auto-Edit/Full-Auto）体験 | 3段階モード切替, /permissions | 1〜2時間 |
| **Phase 6** | Subagent応用 + /status活用 | 並列Subagent, 設計→実装フロー | 2〜3時間 |
| **Phase 7** | Subagent並列実行・Worktree | 並列エージェント, Worktree隔離 | 3時間 |
| **Phase 8** | codex exec + CI/CD自動化 | codex exec, GitHub Actions | 3〜4時間 |
| **Phase 9** | 総仕上げ + v1.0.0リリース | config.toml最終整備, Release | 3〜4時間 |

---

## 必須習得項目

| 項目 | 習得目標 |
|-----|---------|
| **カスタムコマンド** | `/commit`, `/commit-push-pr`, `/revise-agents-md` を使いこなし、カスタムコマンドを自作できる |
| **Subagent** | Codex CLIのSubagentで独立エージェントを起動し、調査・実装を並列委譲できる |
| **承認モード** | Suggest / Auto-Edit / Full-Auto の3段階を状況に応じて使い分けられる |
| **Suggest Mode** | 設計フェーズで提案のみを確認してから実装するフローを習慣化できる |
| **コミュニケーション保存** | AGENTS.md + `codex resume` + セッション管理の3層でCodex対話を引き継げる |
| **codex exec** | CI/CDスクリプトとしてCodex CLIを自動実行できる |

---

---

# 事前準備（手動インストール・Codex CLI不使用）

**このセクションのみ人間が手動で実行します。**

> **使用するターミナル:** まずPowerShellでWSL2をセットアップし、以降はWSL2（Ubuntu 22.04）のターミナルで作業します。

---

## Step 1: WSL2 + Ubuntu 22.04 のインストール

**PowerShellで実行（管理者権限不要）:**

```powershell
wsl --install -d Ubuntu-22.04
```

インストール後:
1. スタートメニューから「Ubuntu 22.04」を起動
2. ユーザー名とパスワードを設定（Linux用のユーザー）
3. 以降のすべての操作はUbuntuターミナルで実行

**WSL2が既にインストール済みの場合の確認:**
```powershell
wsl --list --verbose   # Ubuntu-22.04 が Version 2 で表示されれば成功
```

**WSL2内でパッケージを最新化:**
```bash
# Ubuntu（WSL2）ターミナルで実行
sudo apt update && sudo apt upgrade -y
```

> **Windows Terminal推奨:** Microsoft StoreからWindows Terminalをインストールすると、WSL2とPowerShellのタブを切り替えながら作業できて便利です。

---

## Step 2: VS Code のインストール（Windowsネイティブ）

1. `https://code.visualstudio.com/` を開く
2. **Download for Windows** をクリック
3. ダウンロードした `.exe` を実行してインストール
   - インストール中「PATHへの追加」オプションを **必ずチェックする**
4. VS Codeを起動して動作確認

**VS Code の Remote - WSL 拡張機能をインストール:**
1. VS Code の拡張機能パネルを開く（`Ctrl+Shift+X`）
2. 「Remote - WSL」または「WSL」と検索してインストール
3. これによりVS CodeからWSL2内のファイルを直接編集できるようになる

```bash
# WSL2ターミナルから VS Code を開く（Remote - WSL経由）
code .   # カレントディレクトリをVS Codeで開く
```

---

## Step 3: WSL2内のgitセットアップ

```bash
# WSL2（Ubuntu）ターミナルで実行
git --version   # 通常はプリインストール済み

# なければインストール
sudo apt install git -y

# ユーザー設定
git config --global user.name "あなたの名前"
git config --global user.email "あなたのメール"
```

---

## Step 4: OpenAI アカウントの作成

1. `https://platform.openai.com/` を開いてアカウントを作成
2. APIキーを取得: `https://platform.openai.com/api-keys` → 「Create new secret key」
3. APIキーを安全な場所にメモしておく（一度しか表示されない）

---

## Step 5: WSL2内に Node.js をインストール（nvm経由）

```bash
# WSL2（Ubuntu）ターミナルで実行

# nvm インストール
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc

# Node.js LTS版をインストール
nvm install --lts
nvm use --lts

# 確認
node --version   # v20.x.x などが表示されれば成功
npm --version
```

---

## Step 6: Codex CLI のインストール

```bash
# WSL2（Ubuntu）ターミナルで実行
npm i -g @openai/codex

# 確認
codex --version   # バージョンが表示されれば成功

# OpenAI APIキーを設定
export OPENAI_API_KEY="sk-..."   # 取得したAPIキー

# 永続化（~/.bashrc または ~/.zshrc に追記）
echo 'export OPENAI_API_KEY="sk-..."' >> ~/.bashrc
source ~/.bashrc
```

---

## Step 7: gh CLI のインストール

```bash
# WSL2（Ubuntu）ターミナルで実行
sudo apt install gh -y
gh auth login   # ブラウザでGitHub認証
```

---

## ✅ 事前準備 完了チェック

- [ ] WSL2（Ubuntu 22.04）が起動できる
- [ ] `code .` でVS Codeが Remote - WSL モードで開く
- [ ] `git --version` が動作する（WSL2内）
- [ ] `node --version` が v18以上（WSL2内）
- [ ] `codex --version` が動作する（WSL2内）
- [ ] `echo $OPENAI_API_KEY` でAPIキーが表示される
- [ ] `gh auth status` → 認証済みと表示される

**→ 以降はすべてWSL2（Ubuntu）ターミナルでCodex CLIを使って進めます**

---

---

# Phase 0: WSL2環境構築 + Python環境【Codex CLI 半自動セットアップ】

**このフェーズで習得すること:**
- Codex CLIによる環境診断・半自動セットアップの体験
- uv（Python管理ツール）による仮想環境構築
- AGENTS.mdの作成（Codex CLIのコンテキスト引き継ぎファイル）
- GitHubリポジトリの初期化

**セッション開始:** WSL2（Ubuntu）ターミナルで `codex` を起動

> **Agent活用の原則（このフェーズから意識する）**
> メインセッションは「指示を出す」ことに専念し、実行はCodex CLIに任せる。
> コマンドは自分で打たず、Codex CLIのツール実行許可ダイアログで「許可」するだけ。

> **AGENTS.md とは:**
> Codex CLIが自動で読み込むプロジェクト設定ファイル。Claude CodeのCLAUDE.mdに相当する。
> セッション間の「引き継ぎノート」として機能し、次のセッションでもコンテキストが継続される。

---

## ツール方針（企業利用・ライセンス安全基準）

| ツール | ライセンス | 商用利用 | 役割 |
|-------|----------|---------|-----|
| **uv** | MIT / Apache 2.0 | ✅ 制限なし | Python管理 + 仮想環境 + パッケージ管理 |
| **venv** | PSF（Python標準添付） | ✅ 制限なし | フォールバック用 |
| **gh CLI** | MIT | ✅ 制限なし | GitHub操作 |
| ~~conda / Anaconda~~ | 商用有償 | ❌ 大企業は要契約 | **使用禁止** |

---

## タスク P0-0: WSL2環境の最終確認とディレクトリ構造の作成

Codex CLIを起動する前に、WSL2内で以下を確認・実行する:

```bash
# WSL2（Ubuntu）ターミナルで実行
# 作業ディレクトリの作成
mkdir -p ~/study/workspace/learn_claude
cd ~/study/workspace/learn_claude

# WSL2からWindowsのファイルシステムにアクセスする場合のパス確認
# （必要に応じて）Windowsのファイルは /mnt/c/Users/<username>/ からアクセス可能
ls /mnt/c/Users/

# Codex CLIを起動
codex
```

## タスク P0-1: 環境診断をCodex CLIに依頼

```
これからPython開発環境をWSL2（Ubuntu 22.04）上でゼロから構築します。
クリーンなUbuntu環境を想定して、必要なツールを順番に診断してください。

チェック項目（bashで確認）:
1. uv --version（~/.local/bin/uv を確認）
2. python3 --version
3. gh --version（GitHub CLI）

結果をリストアップして、不足しているものとインストール方法を教えてください。
インストールコマンドは私の確認を取ってから1つずつ実行してください。
企業利用でライセンス問題のないツールのみ使用すること。
bashコマンドで実施してください。
```

## タスク P0-2: uv のインストール

Codex CLIが以下を提案するので承認するだけ:

```bash
# WSL2（Ubuntu）での uv インストール
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc
uv --version   # 確認
```

## タスク P0-3: gh CLI のインストール（未インストールの場合）

```bash
# Ubuntu（WSL2）での gh CLI インストール
sudo apt install gh -y
gh auth login   # ブラウザでGitHub認証
```

## タスク P0-4: Python + 仮想環境のセットアップ

```
uvを使ってtaskrプロジェクトのPython環境をWSL2（Ubuntu）上でセットアップしてください。

順番に実行してください（各ステップ前に確認を取ること）:
1. uv python install 3.12
2. mkdir -p ~/study/workspace/learn_claude/taskr
3. cd ~/study/workspace/learn_claude/taskr
4. uv init --python 3.12
5. uv venv
6. uv add click
7. uv add --dev pytest ruff
8. uv run python --version で動作確認
9. pyproject.tomlの内容を表示して確認

各コマンドは確認を取ってから実行してください。
```

## タスク P0-5: GitHubリポジトリ作成 + config.toml の初期設定

```
以下を順番に実施してください（WSL2のbash環境で）:

1. gh repo create learn_claude --public でGitHubリポジトリを作成
2. ~/study/workspace/learn_claude/ で git init
3. git remote add origin [リポジトリURL]
4. ~/.codex/config.toml を以下の内容で作成:

---
model = "o4-mini"
approval_policy = "suggest"

[notify]
on_task_complete = ["bash", "-c", "echo 'Task complete: $(date)'"]
---

5. AGENTS.md を以下の内容で作成:

---
# taskr プロジェクト

## 概要
Codex CLI バイブコーディング学習プロジェクト（WSL2版）。
Python CLIタスク管理ツール「taskr」を構築しながらCodex CLIを習得する。

## Python環境
- Pythonバージョン: 3.12
- パッケージ管理: uv
- 仮想環境: taskr/.venv/
- 実行方法: uv run <command>
- OS: WSL2（Ubuntu 22.04）

## 現在のフェーズ
Phase 0 完了 → Phase 1 へ

## 設計決定ログ
（各フェーズで追記していく）
---

各ステップの結果を報告しながら進めてください。
```

> **config.toml について:**
> `~/.codex/config.toml` は Codex CLI のグローバル設定ファイル。
> Claude Code の `~/.claude/settings.json` に相当する。
> `approval_policy` で承認モード（suggest/auto-edit/full-auto）を設定する。

## タスク P0-6: .gitignore と .codexignore の作成

```
WSL2（Ubuntu）開発環境向けに .gitignore と .codexignore を作成してください。

.gitignore に含めるもの:
- .venv/
- __pycache__/
- *.pyc
- .pytest_cache/
- *.egg-info/
- dist/
- .DS_Store      （macOSファイル、念のため）
- Thumbs.db      （Windowsサムネイル）
- .codex/        （ローカル設定）

.codexignore に含めるもの:
- .venv/
- __pycache__/
- .pytest_cache/
- *.pyc

.codexignore は Claude Code の .claudeignore に相当するファイルです。
なぜこれらを除外するのかも説明してください。
```

---

## ✅ Phase 0 完了チェック

- [ ] `uv run python --version` → Python 3.12.x（WSL2内）
- [ ] `uv run pytest --version` → pytestバージョンが表示される
- [ ] `gh auth status` → 認証済みと表示される
- [ ] `AGENTS.md` が作成済み
- [ ] `~/.codex/config.toml` が作成済み
- [ ] GitHubリポジトリが作成済み

## Phase 0 コミット & プッシュ【手動操作】

Phase 0はmainブランチへ直接コミット（初期セットアップのため）。

```bash
git add AGENTS.md .gitignore .codexignore taskr/pyproject.toml
git commit -m "feat(phase0): initial project setup with Python 3.12 via uv on WSL2"
git push origin main
```

セッション管理:
```
/status
```
→ 現在のモデル・トークン使用量・セッション状態を確認してからセッションを終了する。

次のセッション再開時:
```bash
codex resume --last   # 最後のセッションを再開
# または
codex   # 新しいセッション（AGENTS.mdが自動読込される）
```

> **セッション管理のメモ:** Codex CLIでは `codex resume --last` で最後のセッションを再開できる。Claude Codeの `/rename` + `/resume` に相当する機能。

**→ ここでCodex CLIセッションを終了して、新しいセッションでPhase 1を開始する**

---

---

# Phase 1: 基本対話・Suggest Mode・コミュニケーション保存

**このフェーズで習得すること:**
- スラッシュコマンド（/quit, /status, /permissions, /diff, /mention, /model）
- Suggest Mode初体験（設計確認から実装へのフロー）
- AGENTS.mdへの保存習慣（コミュニケーション保存）
- セッション管理（`codex resume` / `/resume`）

**セッション開始:** 新しいWSL2ターミナルで `codex` を起動。AGENTS.mdが自動読込されることを確認する。

---

## タスク P1-1: スラッシュコマンドの探索

以下を順番に試す:

```
/status
```
→ 現在のモデル・承認モード・トークン使用量を確認する

```
/model
```
→ 利用可能なモデル一覧を確認し、`o4-mini` ↔ `gpt-4o` を切り替えてみる

```
/permissions
```
→ 現在の承認モードを確認する

```
/diff
```
→ 現在のGit差分を確認する

- `Shift+Enter` で複数行入力を試す

> **Codex CLIの主なスラッシュコマンド:**
> - `/quit`, `/exit` - セッション終了
> - `/status` - 状態・モデル・トークン使用量確認
> - `/permissions` - 承認モード変更（suggest/auto-edit/full-auto）
> - `/diff` - Gitの差分確認
> - `/mention [path]` - ファイルをコンテキストに追加
> - `/resume` - 保存済みセッションの再開
> - `/fork` - セッションを分岐
> - `/model` - モデル切り替え

## タスク P1-2: Suggest Mode 初体験（Plan Modeの代替）

> **Codex CLIにPlan Modeはない。**
> 代わりに承認モード `suggest` を使う。`suggest` モードでは、Codexが提案（差分）を表示するだけで、自動実行はしない。ユーザーが内容を確認してから `y` で適用する。
> Claude CodeのPlan Modeに相当する「設計→確認→実装」の流れをこのモードで体験する。

```
/permissions
```

→ `suggest` モードに設定されていることを確認する。

Suggest Mode下で以下を投げる:
```
taskrの基本設計を提案してください（コードはまだ書かない）。

検討事項:
- データモデル（Taskの属性: id, title, done, created_at, tags）
- 保存形式（JSONを選定済み。その理由を整理する）
- CLIコマンド一覧（add, list, done, delete）
- ファイル構成（src layout vs flat layout）

設計書としてまとめ、私が確認してから次に進みます。
WSL2（Ubuntu）環境での実行を前提にしてください。
```

→ 設計書を確認して内容を承認する。コードの適用はまだしない。

## タスク P1-3: コミュニケーション保存（AGENTS.md更新）

```
Suggest Modeで確認した設計内容をAGENTS.mdに記録してください。

「設計決定ログ」セクションに以下を追記:
- 決定したデータモデル
- 保存形式の選定理由
- CLIコマンド一覧
- ファイル構成の決定

/mention AGENTS.md でファイルをコンテキストに追加してから編集してください。
```

> **コミュニケーション保存の3層構造**
> ```
> Layer 1: AGENTS.md        ← 長期保存（プロジェクト全体の知識・方針）
> Layer 2: codex resume     ← 中期保存（セッション履歴の再開）
> Layer 3: /status確認      ← セッション管理（状態を把握してから終了する）
> ```

## タスク P1-4: /mention コマンドの活用

```
/mention ~/study/workspace/learn_claude/taskr/pyproject.toml
```

→ ファイルをコンテキストに追加してから、内容について質問・分析する体験をする。

```
追加したpyproject.tomlの内容を確認して、
taskrプロジェクトに必要な依存関係が揃っているか評価してください。
```

## タスク P1-5: セッション管理の練習（codex resume を理解する）

> **`codex resume` とは:**
> 過去のCodex CLIセッションを再開するコマンド。
> `codex resume --last` で最後のセッション、`codex resume` でセッション一覧から選択できる。
> Claude Codeの `/rename` + `/resume` に相当する機能。
>
> **なぜセッションを分けるか:**
> Codex CLIセッションを終了すると会話履歴は引き継がれない。
> `codex resume` でその時点の文脈を呼び戻して作業を再開できる。
> AGENTS.mdが「**何を**作っているか」を伝えるなら、
> セッション履歴は「**どこまで**作業したか」を伝えるブックマーク。

以下を試す:
```
/status
```
→ 現在のセッション状態を記録してから終了する。

セッション終了 → `codex resume --last` で再開する体験をする。

---

## ✅ Phase 1 完了チェック

- [ ] Suggest Modeで設計書を作成できた
- [ ] AGENTS.mdの設計決定ログが更新された
- [ ] `codex resume --last` でセッションを再開できた
- [ ] `/mention` でファイルをコンテキストに追加できた

## Phase 1 コミット & プッシュ【手動・featureブランチ初体験】

```bash
git checkout -b feature/phase1-design
git add AGENTS.md
git commit -m "docs(phase1): add design decisions and taskr architecture plan"
git push origin feature/phase1-design
gh pr create \
  --title "docs(phase1): add design decisions" \
  --body "Suggest Modeで確認した設計内容をAGENTS.mdに記録" \
  --base main
gh pr merge --merge
git checkout main
git pull origin main
```

```
/status
```
→ セッション状態を確認してから終了する。

**→ セッション終了。新しいセッションでPhase 2を開始する**

---

---

# Phase 2: 初期実装 + git統合 + カスタムコマンド初体験

**このフェーズで習得すること:**
- バイブコーディングの基本リズム（対話 → 確認 → 適用 → 検証）
- カスタムコマンド（`/commit`）初体験
- gh CLI基本操作

**セッション開始:** 新しいセッション。

> **Codex CLIのカスタムコマンドとは:**
> `~/.codex/<name>.md` に定義した再利用可能なプロンプトテンプレート。
> `/name` で呼び出せるショートカットコマンド。
> Claude Codeの Skillsフォルダ（`~/.claude/skills/<name>/SKILL.md`）に相当する。

**フェーズ開始時にブランチを作成（手動）:**
```bash
git checkout -b feature/phase2-core-impl
```

---

## タスク P2-1: プロジェクト構造の作成

```
taskrプロジェクトのディレクトリ構造を作成してください。
WSL2（Ubuntu）のbash環境で実行してください。

taskr/
  taskr/
    __init__.py
    cli.py      （clickベースのエントリポイント）
    models.py   （Taskデータクラス）
    storage.py  （JSON読み書き）
  tests/
    __init__.py
    test_models.py
    test_storage.py
  pyproject.toml（既存）
  README.md
```

## タスク P2-2: バイブコーディングで実装

以下を **順番に** 投げる（次を投げる前に動作確認する）:

```
# Step 1: データモデル
models.py に Task dataclass を実装してください。
フィールド: id(UUID), title(str), done(bool), created_at(datetime), tags(list[str])
JSON変換メソッド（to_dict / from_dict）も含めてください。
pathlib.Path でパス処理を行ってください（WSL2のLinuxパスを使用）。
```

```
# Step 2: ストレージ（Step 1確認後）
storage.py にJSON永続化を実装してください。
保存先: ~/.taskr/tasks.json（WSL2のホームディレクトリ）
pathlib.Path を使ってパス処理をしてください。
CRUD操作: create, list_all, update, delete
```

```
# Step 3: CLIエントリポイント（Step 2確認後）
cli.py にclickを使ったCLIを実装してください。
コマンド: add, list, done, delete
まず add だけ実装して動作確認してから残りを追加してください。
```

```bash
# 動作確認（WSL2内）
uv run python -m taskr add "最初のタスク"
uv run python -m taskr list
```

## タスク P2-3: テストの作成

```
tests/test_storage.py に pytest のテストを書いてください。
tmp_path フィクスチャで一時ディレクトリを使い、
~/.taskr/ の本番データに影響しないようにしてください。
カバー範囲: タスク追加・取得・更新・削除の4操作
```

```bash
uv run pytest   # テストが通ることを確認
```

## タスク P2-4: カスタムコマンド初体験（/commit 相当）

まずカスタムコマンドを作成する:

```
~/.codex/commit.md というファイルを作成してください。
内容は以下のカスタムコマンド定義:

---
# commit カスタムコマンド

git diff --staged の差分を分析して、適切なConventional Commits形式の
コミットメッセージを提案してください。

形式: <type>(<scope>): <subject>

typeの種類:
- feat: 新機能
- fix: バグ修正
- docs: ドキュメント
- refactor: リファクタリング
- test: テスト追加・修正
- chore: ビルド・設定変更

提案後、ユーザーの承認を得てから git commit を実行してください。
---

作成後、/commit で呼び出して動作確認してください。
```

```
/commit
```

→ Codexが変更差分を分析してコミットメッセージを提案してくれる。
→ 内容を確認して承認する。

## タスク P2-5: gh CLI でIssueを作成

```
gh CLIを使って以下のIssueをGitHubに作成してください:
タイトル: "Phase 3: Hooks設定を実施する"
ラベル: enhancement
本文: "notify hookでタスク完了時に通知、カスタムコマンドを自作する設定を追加する"
```

---

## ✅ Phase 2 完了チェック

- [ ] `uv run python -m taskr add "テスト"` が動作する（WSL2内）
- [ ] `uv run pytest` でテストがすべてパスする
- [ ] `/commit` カスタムコマンドを使ってコミットできた

## Phase 2 コミット & プッシュ【/commit + 手動push/PR】

```bash
git add -u
```

```
/commit
```

```bash
git push origin feature/phase2-core-impl
gh pr create \
  --title "feat(phase2): implement core taskr CLI" \
  --body "add/list/done/deleteコマンドとJSONストレージを実装。pytestテスト付き。" \
  --base main
gh pr merge --merge
git checkout main && git pull origin main
```

```
/status
```
→ セッション状態を確認してから終了する。

**→ セッション終了。新しいセッションでPhase 3を開始する**

---

---

# Phase 3: Hooks設定 + カスタムコマンド作成

**このフェーズで習得すること:**
- `config.toml` の `[notify]` フックによるタスク完了通知
- WSL2環境向けの通知設定（notify-send または echo）
- カスタムコマンドの自作（`~/.codex/<name>.md`）
- .codexignore の整備

**セッション開始:** 新しいセッション。

**フェーズ開始時にブランチを作成（手動）:**
```bash
git checkout -b feature/phase3-hooks-custom-cmd
```

---

## タスク P3-1: notify フックの設定（タスク完了通知）

```
~/.codex/config.toml に notify フックを追加してください。
WSL2（Ubuntu）環境向けの設定で実装してください。

notify設定1: タスク完了時にターミナルに通知メッセージを表示
  コマンド: bash -c "echo 'Codex: Task complete at $(date)'"

notify設定2: notify-send が使える場合はデスクトップ通知も追加
  まず notify-send --version で確認し、なければインストール:
  sudo apt install libnotify-bin -y

設定後、簡単なタスクを実行してフックが動くか確認してください。
```

> **WSL2での通知について:**
> macOSの `osascript` はWSL2では使えない。
> WSL2ではターミナルへの `echo` 出力か `notify-send` を使う。
> `notify-send` を使うにはWSL2の GUI統合（WSLg）が有効である必要がある。

## タスク P3-2: config.toml の高度な設定

```
~/.codex/config.toml を以下の設定で拡張してください:

1. デフォルトモデルを o4-mini に設定（既に設定済みか確認）
2. 承認モードを suggest に設定（既に設定済みか確認）
3. 作業ディレクトリのデフォルトを ~/study/workspace/learn_claude/ に設定
4. 設定の全オプションと意味を説明してください

設定後、codex を再起動して設定が反映されているか /status で確認してください。
```

## タスク P3-3: PreToolUse相当の安全設定

```
Codex CLIで危険なコマンドを防ぐための設定を実装してください。

~/.codex/safety-check.md というカスタムコマンドを作成してください:
- 危険なコマンド（rm -rf, sudo rm, データベース削除など）を
  実行しようとした場合に警告を出す
- 実行前に必ず確認を求める

また、config.toml で自動実行を制限する設定があれば追加してください。
```

## タスク P3-4: .codexignore の整備

```
taskr/.codexignore を確認・更新してください。
WSL2（Ubuntu）環境で除外すべきパスとその理由を説明してください。

追加で除外すべきもの:
- .venv/（仮想環境）
- __pycache__/（Pythonキャッシュ）
- .pytest_cache/（テストキャッシュ）
- *.pyc
- .git/（gitデータ）
```

## タスク P3-5: カスタムコマンドの自作【カスタムコマンド必須】

```
カスタムコマンドを作成してください。

ファイル: ~/.codex/taskr-status.md
内容:
---
# taskr-status カスタムコマンド

taskrプロジェクトの現在の実装状況とテスト結果を確認して報告する。

以下を確認してレポートせよ:
1. uv run pytest --tb=short の結果
2. uv run ruff check . の結果
3. 実装済みのCLIコマンド一覧
4. AGENTS.mdの「現在のフェーズ」を確認して次にやるべきことを提示
---

カスタムコマンド作成後、/taskr-status で呼び出して動作確認してください。
```

## タスク P3-6: セッション終了の半自動化【/end-phase カスタムコマンド】

毎フェーズ末のセッション管理を半自動化します。

```
以下のカスタムコマンドを作成してください。

ファイル: ~/.codex/end-phase.md
内容:
---
# end-phase カスタムコマンド

フェーズ完了時のセッション管理を補助する。
現在の作業内容を整理し、AGENTS.mdの現在フェーズを更新する。

以下のステップで処理せよ:

1. AGENTS.md を /mention で読んで「現在のフェーズ」と直近の作業内容を確認する
2. 以下の情報を整理して出力する:

   セッション終了前の確認事項:
   - 現在のフェーズ: [フェーズ名]
   - 完了した作業: [箇条書き]
   - 次のフェーズ: [次フェーズ名]
   - 再開コマンド: codex resume --last

3. AGENTS.mdの「現在のフェーズ」を次のフェーズに更新してよいか確認する。
   OKなら自動で更新する。

4. /status で現在のトークン使用量を確認してレポートする。
---

カスタムコマンド作成後、/end-phase を実行して動作確認してください。
```

> **このカスタムコマンドの効果:**
> - フェーズ終了時の作業確認が体系化される
> - AGENTS.mdの「現在のフェーズ」更新も忘れなくなる
> - **Phase 3以降はすべてのフェーズ末に `/end-phase` を使う**

---

## ✅ Phase 3 完了チェック

- [ ] タスク完了時にnotifyフックが動作する
- [ ] `/taskr-status` カスタムコマンドが動作する
- [ ] `/end-phase` カスタムコマンドがフェーズ情報を整理してくれる
- [ ] `~/.codex/config.toml` が適切に設定されている

## Phase 3 コミット & プッシュ【/commit + 手動push/PR（定着）】

```bash
git add taskr/.codexignore AGENTS.md
# ~/.codex/ の個人設定はリポジトリに含めない
```

```
/commit
```

```bash
git push origin feature/phase3-hooks-custom-cmd
gh pr create \
  --title "feat(phase3): add codexignore and update AGENTS.md" \
  --body "notify hook設定、カスタムコマンド(/taskr-status, /end-phase)を自作。" \
  --base main
gh pr merge --merge
git checkout main && git pull origin main
```

```
/end-phase
```
→ Codexがフェーズ情報を整理してくれる。AGENTS.mdを更新してセッションを終了する。

**→ セッション終了。新しいセッションでPhase 4を開始する**

---

---

# Phase 4: MCP統合 + Subagent 初体験

**このフェーズで習得すること:**
- `config.toml` への MCP設定
- GitHub MCP によるIssue/PR管理
- SubagentによるCodex CLI並列調査の基本

**セッション開始:** 新しいセッション。

> **Codex CLIでのSubagentとは:**
> Codex CLIのSubagent機能。メインセッションとは独立したコンテキストで
> 調査や実装タスクを並列実行できる。

**フェーズ開始時にブランチを作成（手動）:**
```bash
git checkout -b feature/phase4-mcp-subagent
```

---

## タスク P4-1: GitHub MCP のセットアップ

```
GitHub MCPをCodex CLIにセットアップしてください。

~/.codex/config.toml に以下のMCP設定を追加してください:

[mcp_servers.github]
command = "npx"
args = ["-y", "@modelcontextprotocol/server-github"]
env = { GITHUB_PERSONAL_ACCESS_TOKEN = "$GITHUB_TOKEN" }

事前に必要なこと:
1. GitHub Personal Access Token を取得（Settings → Developer settings → Personal access tokens）
2. ~/.bashrc に export GITHUB_TOKEN="ghp_..." を追加
3. source ~/.bashrc

設定後、以下を確認:
1. GitHub MCPでlearn_claudeリポジトリのIssue一覧を取得
2. Phase 3で作成したIssueをMCP経由でクローズ
```

> **Codex CLIのMCP設定について:**
> Claude Codeが `.mcp.json` を使うのに対し、
> Codex CLIは `~/.codex/config.toml` の `[mcp_servers.<name>]` セクションで設定する。

## タスク P4-2: Subagent 初体験【Subagent必須】

```
Subagent機能を使って調査タスクを委託してください。

依頼内容:
「clickライブラリを使ったCLIのテスト方法（pytestとの組み合わせ）を
 調査した上でベストプラクティスを報告してください。
 特にWSL2（Linux）環境での注意点があれば含めてください。」

Subagentを起動して調査を実行し、結果をメインセッションに返してもらってください。
返ってきた結果をもとに test_cli.py を改善してください。
```

## タスク P4-3: タグフィルタリング機能の実装

```
Subagentの調査結果を活用して、
taskr list --tag <タグ名> でタグ絞り込みができる機能を実装してください。

実装後:
- テストを追加
- uv run pytest で確認
```

## タスク P4-4: /commit-push-pr カスタムコマンド初体験

まずカスタムコマンドを作成する:

```
~/.codex/commit-push-pr.md というカスタムコマンドを作成してください。
内容:
- git diff --staged を分析してコミットメッセージを提案
- 承認後に git commit を実行
- git push origin <現在のブランチ> を実行
- gh pr create でPRを作成（タイトルとdescriptionは変更内容から自動生成）
- 各ステップで確認を取りながら進める

作成後、/commit-push-pr で呼び出して動作確認してください。
```

```
/commit-push-pr
```

→ `add + commit + push + PR作成` がワンコマンドで完了する体験をする。

---

## ✅ Phase 4 完了チェック

- [ ] GitHub MCPでIssueを操作できた
- [ ] Subagentに調査を委託して結果を受け取れた
- [ ] `taskr list --tag work` が動作する
- [ ] `/commit-push-pr` でPRを作成できた

## Phase 4 コミット & プッシュ【/commit-push-pr 初体験】

```
/commit-push-pr
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

```
/end-phase
```
→ フェーズ情報を整理してAGENTS.mdを更新する。

**→ セッション終了。新しいセッションでPhase 5を開始する**

---

---

# Phase 5: 承認モード体験（Suggest / Auto-Edit / Full-Auto）

**このフェーズで習得すること:**
- Codex CLIの3段階承認モードの意味と使い分け
- `/permissions` コマンドによるモード切替
- `config.toml` での `approval_policy` 設定
- 各モードの安全性とリスクの理解

**セッション開始:** 新しいセッション。

**フェーズ開始時にブランチを作成（手動）:**
```bash
git checkout -b feature/phase5-approval-modes
```

---

## タスク P5-1: 3段階承認モードの理解

```
Codex CLIの承認モードについて詳しく説明してください。

説明してほしい内容:
1. suggest モード（デフォルト）
   - どういう動作をするか
   - どんな場面で使うべきか
   - リスクと制限

2. auto-edit モード
   - どういう動作をするか
   - suggest との違い
   - どんな場面で使うべきか

3. full-auto モード
   - どういう動作をするか
   - 最も危険な操作とは何か
   - どんな場面でのみ使うべきか

4. Claude Codeの allow/deny ルールとの比較
   - 設計思想の違い
   - それぞれのメリット・デメリット

各モードの使い方と注意点をまとめてください。
```

## タスク P5-2: suggest モードの体験（設計フェーズの活用）

```
/permissions
```
→ suggest モードを確認・設定する。

```
suggest モードで以下の機能追加を提案してもらってください（実行しない）:

taskrにsearch機能を追加する設計を提案してください。
- taskr search <キーワード> でタイトル・タグを検索
- 大文字小文字を区別しない
- 複数キーワードのAND検索

設計書（変更ファイル・実装方針）を示してください。
コードの適用は私が確認してから行います。
```

→ 提案された差分を確認し、内容が良ければ `y` で適用する。

## タスク P5-3: auto-edit モードの体験（実装フェーズの活用）

```
/permissions
```
→ `auto-edit` モードに切り替える。

```
auto-edit モードで以下を実装してください:
- taskr search の基本実装（タイトル検索のみ）
- tests/test_search.py にテストを追加

各ファイルの変更は自動適用されますが、コマンド実行は確認を求めてください。
実装後 uv run pytest で確認してください。
```

→ ファイル変更が自動で行われることを体験する。

## タスク P5-4: full-auto モードの体験（注意が必要）

> **警告:** full-auto モードはすべての操作を確認なしで実行する。
> 本番環境や重要なリポジトリでの使用は危険。テスト環境や実験的な作業にのみ使用すること。

```
/permissions
```
→ `full-auto` モードに切り替える。

```
full-auto モードで以下を実行してください:
- taskr search のタグ検索を追加（--tag オプション）
- テストを追加

この作業でfull-autoがどのように動作するか観察してください。
作業完了後、suggest モードに戻してください。
```

```
/permissions
```
→ 作業後に `suggest` モードに戻すことを忘れずに。

## タスク P5-5: プロジェクト固有の承認モード設定

```
以下を検討・実装してください:

1. ~/.codex/config.toml のデフォルト承認モードをどれに設定すべきか
   - 理由とともに推奨モードを提案してください

2. 作業フェーズに応じた承認モードの使い分けガイドを
   AGENTS.mdの「開発ガイドライン」セクションに追記してください

3. 危険な操作を防ぐためのベストプラクティスをまとめてください
```

---

## ✅ Phase 5 完了チェック

- [ ] suggest / auto-edit / full-auto の違いを説明できる
- [ ] `/permissions` でモード切替ができた
- [ ] `taskr search` が動作する
- [ ] AGENTS.mdに承認モードの使い分けガイドが記録された

## Phase 5 コミット & プッシュ【/commit-push-pr（定着）】

```
/permissions
```
→ `suggest` モードに戻してから作業する。

```
/commit-push-pr
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

```
/end-phase
```
→ フェーズ情報を整理してAGENTS.mdを更新する。

**→ セッション終了。新しいセッションでPhase 6を開始する**

---

---

# Phase 6: Subagent応用 + /status活用

**このフェーズで習得すること:**
- `/status` で設計状況を確認しながら進めるフロー（Plan Modeの代替）
- Subagentを並列で動かして設計を補強する
- 設計判断の文書化
- `/fork` によるセッション分岐

**セッション開始:** 新しいセッション。

---

## タスク P6-1: /status + Suggest Modeで大機能を設計

```
/permissions
```
→ `suggest` モードを確認する。

```
/status
```
→ 現在のモデルとコンテキスト状況を確認する。

```
taskrに以下の機能を追加する計画を提案してください（コードはまだ書かない）:
- 優先度システム (high / medium / low)
- 期限設定 (due date、YYYY-MM-DD形式)

設計書に含めること:
- データモデルの変更（Taskクラスへの追加フィールド）
- 既存データとの後方互換性戦略（既存のtasks.jsonが壊れない方法）
- CLI引数の変更（add --priority high --due 2026-03-01 など）
- テストの追加計画
- 実装の順序
WSL2（Ubuntu）環境での実行を前提にしてください。
```

→ 設計書を確認して内容を承認する。

## タスク P6-2: Subagentによる並列調査【Subagent応用】

```
設計判断を補強するため、Subagentを2つ並列で動かしてください。

Subagent 1:
「CLIツールにおける優先度実装のベストプラクティスを調査してください」

Subagent 2:
「Pythonのdatetime/date型の扱い方とJSONシリアライズ方法を
 WSL2（Ubuntu）環境で調査してください」

2つのSubagentの調査結果をまとめて、設計書を最終化してください。
```

## タスク P6-3: /fork でブランチセッションを作成

```
/fork
```

→ セッションをフォーク（分岐）する体験をする。
→ フォークされたセッションで実装の一部を試す。

```
フォークしたセッションで以下のみ実装してみてください（全体の実装前の試験）:
- models.py に priority フィールドのみ追加
- JSON変換メソッドを更新
- 動作確認

メインセッションには影響しないことを確認してください。
```

## タスク P6-4: 実装（auto-edit モード使用）

```
/permissions
```
→ `auto-edit` モードに切り替える。

```
設計書に従って優先度と期限機能を実装してください。
実装順序: models.py → storage.py → cli.py → テスト追加
各ステップで uv run pytest を実行して既存テストが壊れていないか確認してください。
```

```
/permissions
```
→ 実装完了後、`suggest` モードに戻す。

## タスク P6-5: 設計判断をAGENTS.mdに記録

```
AGENTS.mdの「設計決定ログ」に以下を追記してください:
- 優先度・期限機能の設計方針とその理由
- 後方互換性の解決方法
- 設計書から変更した点と理由
```

---

## ✅ Phase 6 完了チェック

- [ ] Suggest Modeで設計書を作成できた
- [ ] Subagentを2つ並列で動かせた
- [ ] `/fork` でセッション分岐を体験した
- [ ] `taskr add "タスク" --priority high --due 2026-03-15` が動作する
- [ ] AGENTS.mdに設計判断が記録された

## Phase 6 コミット & プッシュ【プロンプト一発でブランチ〜PR（フル自動化）】

```
feature/phase6-priority-duedate ブランチを作成して、
今フェーズの実装内容をコミット・プッシュして、PRを作成してください。
PRのタイトルとdescriptionは変更内容から自動生成してください。
bash環境でgitコマンドを実行してください（WSL2内）。
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

```
/end-phase
```
→ フェーズ情報を整理してAGENTS.mdを更新する。

**→ セッション終了。新しいセッションでPhase 7を開始する**

---

---

# Phase 7: Subagent並列実行・Worktree

**このフェーズで習得すること:**
- Subagentの並列起動と協調
- Worktreeを使った隔離実験
- バックグラウンドSubagent実行
- 複数Subagentの結果統合

**セッション開始:** 新しいセッション。

---

## タスク P7-1: Subagent並列実行の仕組みを理解

```
Codex CLIのSubagent機能について詳しく説明してください:
1. どのような仕組みで動くか
2. 並列実行の方法
3. Worktree隔離とは何か
4. WSL2環境での注意点があれば教えてください
5. このtaskrプロジェクトでSubagentを使うのに適したシナリオを3つ提案してください
```

## タスク P7-2: 2Subagent並列実行体験【Subagent並列必須】

```
Subagentを2つ並列で起動して以下を実行してください:

Subagent 1（テスト担当）:
- test_models.py の作成（Taskクラスの全メソッドをテスト）
- test_storage.py の更新（priority, due_dateのテストを追加）

Subagent 2（ドキュメント担当）:
- README.md の更新（全コマンドの使い方を追記）
- CHANGELOG.md の作成（バージョン履歴の開始）

2つのSubagentが完了したら結果を統合して、テストが通ることを確認してください。
```

## タスク P7-3: Worktreeを使った実験

```
Worktree機能を使って、SQLite移行の実験をしてください:

1. feature/sqlite-storage ブランチのWorktreeを作成
2. そのWorktreeでstorage.pyをSQLiteに変更する実験を行う
3. メインブランチはJSONのまま維持する
4. 実験完了後、JSONとSQLiteの比較結果を報告する
5. 今回はJSONを採用（実験のみ、mergeしない）
```

## タスク P7-4: バックグラウンドSubagent実行

```
Subagentをバックグラウンドで実行してください:

バックグラウンドで実行:
- uv run pytest --cov=taskr でカバレッジ計測
- uv run ruff check . でリント全体チェック

メインセッションでは:
- バックグラウンド完了を待ちながら、AGENTS.mdにPhase 7の学習記録を追記する

バックグラウンドSubagentが完了したら結果を確認してください。
```

---

## ✅ Phase 7 完了チェック

- [ ] 2Subagentが並列で異なるファイルを編集した
- [ ] Worktreeで実験ブランチを作成・利用できた
- [ ] バックグラウンドSubagent実行を体験した

## Phase 7 コミット & プッシュ【Subagent + フル自動GitHub Flow】

```
Subagentでの作業が完了しました。
feature/phase7-subagent-parallel ブランチを作成して、
テストとドキュメントの変更をまとめてコミット・プッシュ・PR作成してください。
PR本文には「Subagentで並列実装した内容の概要」を自動で記述してください。
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

```
/end-phase
```
→ フェーズ情報を整理してAGENTS.mdを更新する。

**→ セッション終了。新しいセッションでPhase 8を開始する**

---

---

# Phase 8: codex exec + CI/CDスクリプト化

**このフェーズで習得すること:**
- `codex exec` コマンドによるCI/CDスクリプト実行
- GitHub Actionsへの統合
- 複数Subagentの協調とコンフリクト解消
- 自動テスト・自動レポート生成

**セッション開始:** 新しいセッション。

> **codex exec とは:**
> Codex CLIをインタラクティブモードではなくスクリプトとして実行するコマンド。
> Claude Codeにはない機能で、CI/CDパイプラインやバッチ処理に使える。
> `codex exec "タスク内容"` または `codex exec --full-auto "タスク内容"` で実行できる。

---

## タスク P8-1: codex exec の基本体験【codex exec必須】

```
codex exec の使い方を説明してから、以下を実行してください:

1. codex exec の基本的な使い方
   - インタラクティブモード（codex）との違い
   - どんな場面で使うべきか

2. 以下のコマンドをWSL2ターミナルで試す:
   codex exec "uv run pytest の結果を分析して、失敗しているテストがあれば原因を説明してください"

3. 実行結果を確認してください
```

```bash
# WSL2ターミナルで実行（Codex CLIの外から）
codex exec "uv run pytest --tb=short を実行して結果を報告してください"
```

## タスク P8-2: レポート機能の実装【Subagent並列本格活用】

```
Subagentを3つ使って「taskr report」コマンドを実装してください。

要件:
- taskr report で統計情報を表示
- 完了/未完了タスク数、優先度別集計、タグ別集計

Subagent分担:
- Subagent A: storage.pyに集計ロジックを追加
- Subagent B: cli.pyにreportコマンドを追加
- Subagent C: テストとREADME更新

各Subagentの作業完了後に統合してください。
```

## タスク P8-3: コンフリクト解消練習

```
Subagentを使って、意図的にコンフリクトを発生させ、解消を練習してください:

Subagent 1: cli.py に taskr export コマンドを追加（JSON/CSV出力）
Subagent 2: cli.py に taskr import コマンドを追加（JSONインポート）

両Subagentが同じファイルを変更するためコンフリクトが発生します。
コンフリクトの解消プロセスを実践してください。
```

## タスク P8-4: codex exec を使ったCI/CDスクリプト作成

```
codex exec を使って以下のCI/CDスクリプトを作成してください:

1. scripts/ai-code-review.sh を作成:
   - git diff origin/main...HEAD の差分を取得
   - codex exec でコードレビューを実行
   - レビュー結果をファイルに保存

2. scripts/ai-test-report.sh を作成:
   - pytest の実行
   - codex exec でテスト結果を分析・サマリー生成
   - GitHub Actions のサマリーに出力

スクリプト作成後、手動で実行して動作確認してください。
```

## タスク P8-5: GitHub Actions CI設定（WSL2/Linux対応）

```
.github/workflows/ci.yml を作成してください:

トリガー: push, pull_request（mainブランチ）
jobs:
- test: ubuntu-latest でpytest実行（Python 3.11, 3.12 マトリックス）
- lint: uv run ruff check
- coverage: カバレッジ80%以上を要件に
- ai-review: codex exec でAIコードレビューを実行（オプション）

ワークフローファイル作成後にプッシュして、
GitHub MCPでActions実行状況を確認してください。

注意: codex exec を GitHub Actionsで実行する場合は
OPENAI_API_KEY をSecretsに設定が必要です。その手順も含めてください。
```

---

## ✅ Phase 8 完了チェック

- [ ] `codex exec "..."` をWSL2ターミナルから実行できた
- [ ] コンフリクト解消を実践した
- [ ] GitHub ActionsのCIがubuntu-latestで動作している
- [ ] `scripts/ai-code-review.sh` が動作する

## Phase 8 コミット & プッシュ【GitHub MCP + Subagent完全統合】

```
Subagentで実装したすべての変更を
feature/phase8-codex-exec ブランチにまとめてください。
その後:
1. /commit-push-pr でコミット・プッシュ・PR作成
2. GitHub MCPを使ってPRに "enhancement" ラベルを付与
3. PR descriptionにSubagentの作業分担を自動で記録

これをすべてCodex CLIにやってもらう（人間は最終確認のみ）。
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

```
/end-phase
```
→ フェーズ情報を整理してAGENTS.mdを更新する。

**→ セッション終了。新しいセッションでPhase 9を開始する**

---

---

# Phase 9: 総仕上げ【全機能統合 + v1.0.0リリース】

**このフェーズで習得すること:**
- AGENTS.md の最終整備（`/revise-agents-md` カスタムコマンド）
- `~/.codex/config.toml` の最終チューニング
- GitHub Releasesへの公式リリース
- 学習の振り返り

**セッション開始:** 新しいセッション。

---

## タスク P9-1: AGENTS.md の最終整備カスタムコマンド作成

まず `/revise-agents-md` カスタムコマンドを作成する:

```
~/.codex/revise-agents-md.md というカスタムコマンドを作成してください。
内容:
- 現在のAGENTS.mdを読み込んで内容を評価する
- 追加・更新すべき情報を提案する
- 承認後にAGENTS.mdを更新する
- 次のCodex CLIセッションで即座に作業再開できるレベルを目標にする
```

```
/revise-agents-md
```

```
AGENTS.mdを完全版に整備してください:
1. プロジェクト概要と目的
2. WSL2環境のセットアップ手順（環境構築の要約）
3. 全コマンド使い方ガイド（bashコマンド例）
4. アーキテクチャ説明（ファイル構成と役割）
5. 設計決定ログ（全フェーズ分）
6. Codex CLIを使った開発のコツ（WSL2環境での注意点も含む）
7. config.toml の設定説明

次のCodex CLIセッションで即座に開発を再開できるレベルに整えること。
```

## タスク P9-2: config.toml の最終チューニング

```
~/.codex/config.toml を最終版に整備してください:

1. 現在の設定をすべて確認・最適化する
2. 以下の設定が適切か検討する:
   - model: 作業種別に応じた推奨モデル
   - approval_policy: デフォルトモードの最終決定
   - notify: タスク完了通知の最終設定
   - mcp_servers: 設定済みMCPサーバーの確認

3. 学習を通じて最適だと感じた設定をコメント付きで整理する
4. 最終的なconfig.tomlの内容をAGENTS.mdに記録する
```

## タスク P9-3: インタラクティブモードの実装（Subagent総動員）

```
Subagentを複数使って taskr -i（インタラクティブモード）を実装してください。

Subagent（バックグラウンド）:
TUIライブラリの選定調査（questionary, prompt_toolkit, rich を比較して推奨を報告）
WSL2（Ubuntu）のターミナルでの動作確認も含めること

Subagent並列実装:
- Subagent 1: TUIライブラリ導入と基本UI実装
- Subagent 2: キーバインドとナビゲーション
- Subagent 3: 既存CLIとの統合とテスト

uv add でライブラリを追加してから実装してください。
WSL2のターミナルで正しく動作することを確認してください。
```

## タスク P9-4: v1.0.0 リリース

```
taskr v1.0.0 をリリースしてください:

1. pyproject.toml のバージョンを 1.0.0 に設定
2. CHANGELOG.md を完成させる
3. git tag v1.0.0
4. git push origin main --tags
5. GitHub MCPでReleasesページを作成
   タイトル: taskr v1.0.0
   本文: CHANGELOGの内容を使用

/commit-push-pr カスタムコマンドで最終PRを作成してください。
```

## タスク P9-5: codex exec による自動リリースチェック

```
codex exec を使ってリリース前の自動チェックを実行してください:

1. scripts/pre-release-check.sh を作成:
   - uv run pytest でテスト全実行
   - uv run ruff check でリントチェック
   - codex exec でリリースノートの品質チェック
   - codex exec でCHANGELOGの完全性チェック

2. スクリプトを実行してすべてのチェックがパスすることを確認する
```

## タスク P9-6: 学習振り返り

```
このカリキュラムを通じた学びをAGENTS.mdに追記してください。

「学習まとめ」セクション:
1. 各フェーズで習得したCodex CLI機能
2. カスタムコマンド / Subagent / 承認モード それぞれの使いどころと実感
3. バイブコーディングで効果的だったプロンプトパターン
4. codex exec の活用場面と実感
5. 仕事で活用できるフロー（設計確認・コミュニケーション保存）
6. GitHub Flow自動化の段階的変化の振り返り
7. Claude Codeとの違いで感じたこと（良い点・不便な点）
8. WSL2環境で特有のハマりポイントと解決方法

GitHub Issueで今後のロードマップを5件作成してください。
```

---

## ✅ Phase 9 完了チェック

- [ ] `uv run python -m taskr --version` が `1.0.0` を返す
- [ ] GitHub Releasesに v1.0.0 が公開されている
- [ ] AGENTS.mdの学習まとめが完成している
- [ ] `~/.codex/config.toml` が最終版に整備されている
- [ ] `codex exec` による自動チェックスクリプトが動作する

## Phase 9 最終コミット & プッシュ【完全自動化の総仕上げ】

```
以下をすべて自動で実施してください:
1. feature/phase9-final-release ブランチを作成
2. インタラクティブモードの変更をコミット
3. /commit-push-pr でPR作成
4. GitHub MCPでPRを確認・マージ
5. mainにマージ後、git tag v1.0.0 を付与
6. git push origin main --tags
7. GitHub MCPでReleasesを作成（CHANGELOGから自動生成）

bash環境でgitコマンドを実行してください（WSL2内）。
人間が行うのは最終確認のみ。
```

```
/end-phase
```
→ フェーズ情報を整理してAGENTS.mdを最終更新する。

---

---

# カリキュラム完了

おめでとうございます。以下の機能を習得しました:

| 習得項目 | 確認方法 |
|---------|---------|
| **カスタムコマンド** | `/commit`, `/commit-push-pr`, `/revise-agents-md`, `/taskr-status` を作成・活用した |
| **Subagent** | Phase 4, 6, 7, 9 で並列調査・実装を委託した |
| **承認モード** | suggest / auto-edit / full-auto を使い分けた |
| **Suggest Mode** | Phase 1, 6 で設計書を確認してから実装するフローを体験した |
| **コミュニケーション保存** | AGENTS.md + `codex resume` + `/status` で全フェーズを引き継いだ |
| **codex exec** | Phase 8, 9 でCI/CDスクリプトとしてCodex CLIを活用した |
| **GitHub Flow** | 手動操作 → カスタムコマンド自動化 → 完全自動化の変遷を体験した |

**taskr v1.0.0 のリポジトリを公開してカリキュラム完了！**

---

## macOS版（CURRICULUM_CODEX_MAC.md）との主な差異

| 項目 | macOS版 | Windows / WSL2版 |
|-----|--------|-----------------|
| ターミナル | zsh（macOSネイティブ） | WSL2（Ubuntu 22.04）のbash/zsh |
| Codex CLIの実行環境 | macOSネイティブ | WSL2内のLinux環境 |
| インストール前提 | Homebrewが使える | WSL2セットアップが必要（Phase 0冒頭） |
| Node.js インストール | `brew install node` または nvm | nvm経由（WSL2内） |
| gh CLI インストール | `brew install gh` | `sudo apt install gh` |
| uv インストール | `curl -LsSf ... \| sh` | 同じ（WSL2内のbashで実行） |
| 通知フック | `osascript -e '...'` | `echo` または `notify-send`（WSLg必要） |
| VS Code連携 | `code .` でネイティブ起動 | Remote - WSL拡張経由で起動 |
| パス形式 | `~/study/...` | `~/study/...`（WSL2内のLinuxパス） |
| Windowsファイルへのアクセス | 不要 | `/mnt/c/Users/<user>/...` でアクセス可 |
| 環境変数設定 | `~/.zshrc` | `~/.bashrc`（WSL2内） |
| Phase 0の追加タスク | なし | P0-0: WSL2環境確認とディレクトリ作成 |
| CI マトリックス | ubuntu-latest | ubuntu-latest（WSL2と同じLinux環境） |
| codex exec | 同様に使用可能 | WSL2ターミナルから実行 |
