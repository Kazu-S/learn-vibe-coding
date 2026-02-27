# Claude Code バイブコーディング学習カリキュラム（Windows版）

## このドキュメントの使い方

このファイル自体がカリキュラムです。上から順番に読み進め、各タスクのプロンプトをClaude Codeに投げながら学習を進めてください。

**学習スタイル:** バイブコーディング（AIと対話しながらコードを作り上げる）
**制作物:** Python CLIタスク管理ツール「taskr」
**期間目安:** 3週間（週5〜10時間）
**動作環境:** Windows 10 / 11 + PowerShell 7 + Windows Terminal

---

## 変更履歴

| 日付 | 変更内容 |
|------|---------|
| 2026-02-27 | 初版作成（Phase 0〜9、macOS版から移植） |
| 2026-02-27 | `/rename` の意味をPhase 1 P1-5に追加；`/end-phase` スキルをPhase 3 P3-6として追加 |
| 2026-02-27 | 存在しない `/tag` コマンドをすべて削除 |

---

## ⚠️ コンテキスト管理ルール（重要）

**各フェーズは独立したClaude Codeセッションで実施すること。**

フェーズ完了の手順:
1. フェーズ内のタスクをすべて完了
2. featureブランチを commit → push → Pull Request作成（フェーズにより手動/Skills）
3. `/rename` で現在のセッションに名前をつける（例: `/rename phase1-design`）
4. Claude Code セッションを終了
6. 次のフェーズは **新しいセッション** で開始（Claude CodeはCLAUDE.mdを読んでコンテキストを把握する）

**なぜセッションを分けるか:**
- コンテキストウィンドウの使用量を管理するため
- フェーズの境界を明確にするため
- CLAUDE.mdが「セッション間の引き継ぎノート」として機能するため

---

## GitHub Flow の学習ステップ（自動化の段階的移行）

| フェーズ | GitHub Flow | 操作 |
|---------|------------|------|
| Phase 1 | 手動フルフロー | `git checkout -b` → `git add` → `git commit` → `git push` → `gh pr create` |
| Phase 2〜3 | /commit 初体験 | 手動branch + **`/commit`**（メッセージ自動生成）+ 手動push/PR |
| Phase 4〜5 | /commit-push-pr | 手動branch + **`/commit-push-pr`**（add+commit+push+PR一括） |
| Phase 6〜 | 完全自動 | プロンプト一発でbranch〜PRまで全自動 |
| Phase 7〜9 | Agent Teams統合 | 実装完了後に自動PR・自動マージ |

---

## フェーズ一覧

| フェーズ | 内容 | 主な習得機能 | 目安時間 |
|---------|------|------------|---------|
| **事前準備** | VS Code + Claude Code インストール | - | 30分 |
| **Phase 0** | Python環境構築（Claude Code半自動） | uv, gh CLI, GitHub連携 | 1〜2時間 |
| **Phase 1** | 基本対話・プランニング・保存 | Plan Mode, CLAUDE.md, セッション管理 | 2時間 |
| **Phase 2** | 初期実装 + git統合 + Skills | /commit, バイブコーディング | 3時間 |
| **Phase 3** | Hooks + カスタムSkill | PostToolUse Hook, Skill自作 | 2〜3時間 |
| **Phase 4** | MCP + Subagent | GitHub MCP, Task tool | 2〜3時間 |
| **Phase 5** | Permission設定 | allow/deny rules | 1〜2時間 |
| **Phase 6** | Plan Mode本格活用 + Subagent応用 | --plan, 並列Subagent | 2〜3時間 |
| **Phase 7** | Agent Teams基礎 | 並列エージェント, Worktree | 3時間 |
| **Phase 8** | Agent Teams応用 | /feature-dev, CI/CD | 3〜4時間 |
| **Phase 9** | 総仕上げ + v1.0.0リリース | keybindings, Release | 3〜4時間 |

---

## 必須習得項目

| 項目 | 習得目標 |
|-----|---------|
| **Skills** | `/commit`, `/commit-push-pr`, `/revise-claude-md` を使いこなし、カスタムSkillを自作できる |
| **Subagent** | Task toolで独立エージェントを起動し、調査・実装を並列委譲できる |
| **Agent Teams** | 複数エージェントを協調させ、大規模タスクを分散実行できる |
| **Plan Mode** | `--plan` で設計し、承認してから実装するフローを習慣化できる |
| **コミュニケーション保存** | CLAUDE.md + /memory + セッション管理の3層でClaude対話を引き継げる |

---

---

# 🔧 事前準備（手動インストール・Claude Code不使用）

**このセクションのみ人間が手動で実行します。**

> **使用するターミナル:** Windows Terminal + PowerShell 7 を推奨。
> すべての操作を **PowerShell 7** で実施してください。

---

## Step 1: Windows Terminal + PowerShell 7 のインストール

**Windows Terminal:**
1. Microsoft Store を開く
2. 「Windows Terminal」を検索してインストール

**PowerShell 7:**
1. `https://github.com/PowerShell/PowerShell/releases` を開く
2. 最新の `PowerShell-7.x.x-win-x64.msi` をダウンロードしてインストール
3. Windows Terminal を開き、設定（Ctrl+,）→「既定のプロファイル」を「PowerShell」に変更

```powershell
$PSVersionTable.PSVersion  # 7.x.x が表示されれば成功
```

---

## Step 2: VS Code のインストール

1. `https://code.visualstudio.com/` を開く
2. **Download for Windows** をクリック
3. ダウンロードした `.exe` を実行してインストール
   - インストール中「PATHへの追加」オプションを **必ずチェックする**
4. VS Codeを起動して動作確認

```powershell
code --version  # バージョンが表示されれば成功
```

---

## Step 3: Git for Windows のインストール

```powershell
git --version  # インストール済みか確認
```

インストールされていない場合:
1. `https://git-scm.com/download/win` を開く
2. **64-bit Git for Windows Setup** をダウンロードしてインストール
   - 「Use Visual Studio Code as Git's default editor」を選択
   - 「Use Git from the Windows Command Prompt」を選択（デフォルト）
   - 「Checkout Windows-style, commit Unix-style」を選択

```powershell
git --version   # git version 2.x.x が表示されれば成功
git config --global user.name "あなたの名前"
git config --global user.email "あなたのメール"
```

---

## Step 4: Anthropic アカウントの作成

1. `https://claude.ai` を開いてアカウントを作成
2. Claude Codeが利用できるプランに加入（Pro / Max / Team いずれか）
3. ログインしてダッシュボードを確認

---

## Step 5: Node.js のインストール

```powershell
node --version  # v18以上であればOK
```

インストールされていない場合:
1. `https://nodejs.org/` から **LTS版** の Windows Installer (.msi) をダウンロード
2. インストール（デフォルト設定でOK）
3. PowerShell を **再起動** して確認

```powershell
node --version   # v20.x.x などが表示されれば成功
npm --version
```

---

## Step 6: Claude Code CLI のインストール

```powershell
npm install -g @anthropic-ai/claude-code
claude --version   # バージョンが表示されれば成功
claude login       # ブラウザが開く → Anthropicアカウントでログイン
```

---

## Step 7: VS Code の Claude Code 拡張機能をインストール

1. VS Codeを開く
2. 左サイドバーの拡張機能アイコン（四角）をクリック
3. 検索欄に「**Claude Code**」と入力
4. Anthropic製の「Claude Code」拡張機能をインストール
5. VS Codeを再起動
6. 左サイドバーにClaudeアイコンが表示されれば成功

> **補足:** CLIの `claude` コマンドとVS Code拡張は連動します。どちらから操作してもかまいません。

---

## ✅ 事前準備 完了チェック

- [ ] `code --version` が動作する
- [ ] `git --version` が動作する
- [ ] `node --version` が v18以上
- [ ] `claude --version` が動作する
- [ ] `claude` 起動後にログイン済みと表示される
- [ ] VS Code に Claude Code 拡張がインストール済み

**→ 以降はすべてClaude Codeを使って進めます**

---

---

# Phase 0: Python環境構築【Claude Code 半自動セットアップ】

**このフェーズで習得すること:**
- Claude Codeによる環境診断・半自動セットアップの体験
- uv（Python管理ツール）による仮想環境構築
- GitHubリポジトリの初期化

**セッション開始:** Windows Terminal（PowerShell 7）で `claude` を起動

> **Agent活用の原則（このフェーズから意識する）**
> メインセッションは「指示を出す」ことに専念し、実行はClaude Codeに任せる。
> コマンドは自分で打たず、Claude Codeのツール実行許可ダイアログで「許可」するだけ。

---

## ツール方針（企業利用・ライセンス安全基準）

| ツール | ライセンス | 商用利用 | 役割 |
|-------|----------|---------|-----|
| **uv** | MIT / Apache 2.0 | ✅ 制限なし | Python管理 + 仮想環境 + パッケージ管理 |
| **venv** | PSF（Python標準添付） | ✅ 制限なし | フォールバック用 |
| **gh CLI** | MIT | ✅ 制限なし | GitHub操作 |
| ~~conda / Anaconda~~ | 商用有償 | ❌ 大企業は要契約 | **使用禁止** |

---

## タスク P0-1: 環境診断をClaude Codeに依頼

```
これからPython開発環境をWindows上でゼロから構築します。
クリーンなWindows環境を想定して、必要なツールを順番に診断してください。

チェック項目（PowerShellで確認）:
1. uv --version（$env:USERPROFILE\.cargo\bin\uv または $env:USERPROFILE\.local\bin\uv）
2. python --version
3. gh --version（GitHub CLI）

結果をリストアップして、不足しているものとインストール方法を教えてください。
インストールコマンドは私の確認を取ってから1つずつ実行してください。
企業利用でライセンス問題のないツールのみ使用すること。
PowerShellコマンドで実施してください。
```

## タスク P0-2: uv のインストール

Claude Codeが以下を提案するので承認するだけ:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
# PowerShellを再起動後
uv --version   # 確認
```

> **注意:** ExecutionPolicy の変更を求められた場合は `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` を実行してから再試行。

## タスク P0-3: gh CLI のインストール

```powershell
# winget が使える場合（Windows 10 1709以降）
winget install GitHub.cli

# winget がない場合は https://cli.github.com/ からインストーラをDL
gh auth login   # ブラウザでGitHub認証
```

## タスク P0-4: Python + 仮想環境のセットアップ

```
uvを使ってtaskrプロジェクトのPython環境をセットアップしてください。
WindowsのPowerShell環境で実行してください。

順番に実行してください（各ステップ前に確認を取ること）:
1. uv python install 3.12
2. New-Item -ItemType Directory -Path "$env:USERPROFILE\study\workspace\learn_claude\taskr" -Force
3. Set-Location "$env:USERPROFILE\study\workspace\learn_claude\taskr"
4. uv init --python 3.12
5. uv venv
6. uv add click
7. uv add --dev pytest ruff
8. uv run python --version で動作確認
9. pyproject.tomlの内容を表示して確認

各コマンドは確認を取ってから実行してください。
```

## タスク P0-5: GitHubリポジトリ作成 + Agent Teams有効化

```
以下を順番に実施してください（PowerShell環境で）:

1. gh repo create learn_claude --public でGitHubリポジトリを作成
2. "$env:USERPROFILE\study\workspace\learn_claude\" で git init
3. git remote add origin [リポジトリURL]
4. PowerShellプロファイルに環境変数を追加:
   - プロファイルファイルのパスを確認: echo $PROFILE
   - 以下を追記: $env:CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS = "1"
   - プロファイルを再読み込み: . $PROFILE
5. "$env:USERPROFILE\.claude\settings.json" に空のJSON {} を作成
6. CLAUDE.md を以下の内容で作成:

---
# taskr プロジェクト

## 概要
Claude Code バイブコーディング学習プロジェクト（Windows版）。
Python CLIタスク管理ツール「taskr」を構築しながらClaude Codeを習得する。

## Python環境
- Pythonバージョン: 3.12
- パッケージ管理: uv
- 仮想環境: taskr\.venv\
- 実行方法: uv run <command>
- OS: Windows 11 / PowerShell 7

## 現在のフェーズ
Phase 0 完了 → Phase 1 へ

## 設計決定ログ
（各フェーズで追記していく）
---

各ステップの結果を報告しながら進めてください。
```

## タスク P0-6: .gitignore と .claudeignore の作成

```
Windowsの開発環境向けに .gitignore と .claudeignore を作成してください。

.gitignore に含めるもの:
- .venv/
- __pycache__/
- *.pyc
- .pytest_cache/
- *.egg-info/
- dist/
- .DS_Store      （macOSファイル、Windows開発者も念のため）
- Thumbs.db      （Windowsサムネイルキャッシュ）
- desktop.ini    （Windowsフォルダ設定）

.claudeignore に含めるもの:
- .venv/
- __pycache__/
- .pytest_cache/
- *.pyc

なぜこれらを除外するのかも説明してください。
```

---

## ✅ Phase 0 完了チェック

- [ ] `uv run python --version` → Python 3.12.x
- [ ] `uv run pytest --version` → pytestバージョンが表示される
- [ ] `gh auth status` → 認証済みと表示される
- [ ] `CLAUDE.md` が作成済み
- [ ] GitHubリポジトリが作成済み
- [ ] `$env:CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` → `1` が表示される

## Phase 0 コミット & プッシュ【手動操作】

Phase 0はmainブランチへ直接コミット（初期セットアップのため）。

```powershell
git add CLAUDE.md .gitignore .claudeignore taskr/pyproject.toml
git commit -m "feat(phase0): initial project setup with Python 3.12 via uv"
git push origin main
```

セッション管理:
```
/rename phase0-env-setup
```

> **注:** `/rename` の意味はPhase 1のタスクP1-5で詳しく説明します。今は指示通り実行してください。

**→ ここでClaude Codeセッションを終了して、新しいセッションでPhase 1を開始する**

---

---

# Phase 1: 基本対話・プランニング・コミュニケーション保存

**このフェーズで習得すること:**
- スラッシュコマンド（/help, /model, /memory, /rename, /resume）
- Plan Mode初体験（`claude --plan`）
- CLAUDE.mdへの保存習慣
- セッション管理と再開

**セッション開始:** 新しいPowerShellで `claude` を起動。CLAUDE.mdが自動読込されることを確認する。

---

## タスク P1-1: スラッシュコマンドの探索

以下を順番に試す:
```
/help
```
→ コマンド一覧を確認する

```
/model
```
→ Opus 4.6に切り替えてから Sonnet 4.6 に戻す

- `Shift+Enter` で複数行入力を試す
- `Ctrl+S` でプロンプトのスタッシュ/復元を試す

## タスク P1-2: Plan Mode 初体験

新しいPowerShellウィンドウでPlan Modeを起動:
```powershell
claude --plan
```

Plan Mode内で以下を投げる:
```
taskrの基本設計を計画してください（コードは書かない）。

検討事項:
- データモデル（Taskの属性: id, title, done, created_at, tags）
- 保存形式（JSONを選定済み。その理由を整理する）
- CLIコマンド一覧（add, list, done, delete）
- ファイル構成（src layout vs flat layout）

設計書としてまとめ、私が確認してから次に進みます。
```

→ 設計書を確認してPlan Modeを承認（ExitPlanMode）

## タスク P1-3: コミュニケーション保存（CLAUDE.md更新）

```
Plan Modeで決定した設計内容をCLAUDE.mdに記録してください。

「設計決定ログ」セクションに以下を追記:
- 決定したデータモデル
- 保存形式の選定理由
- CLIコマンド一覧
- ファイル構成の決定

その後 /revise-claude-md コマンドも試してください。
```

> **コミュニケーション保存の3層構造**
> ```
> Layer 1: CLAUDE.md    ← 長期保存（プロジェクト全体の知識・方針）
> Layer 2: /memory      ← 中期保存（セッション間で保持したい情報）
> Layer 3: /rename ← セッション管理（後で再開できるように整理）
> ```

## タスク P1-4: /memory コマンドの確認

```
/memory
```

メモリファイルに以下を記録するよう依頼:
```
以下をメモリに保存してください:
- Python 3.12 + uv環境を使っていること（Windows環境）
- taskrプロジェクトの場所（$env:USERPROFILE\study\workspace\learn_claude\taskr\）
- 採用したデータモデルの概要
```

## タスク P1-5: セッション管理の練習（/rename を理解する）

> **`/rename` とは:**
> 現在のセッションに分かりやすい名前をつけるコマンド。
> 名前をつけないと「Session 2026-02-27 14:23」のような自動生成名になり、
> あとで `/resume` で一覧を見たときにどの作業か判別できなくなる。
>
> **なぜ毎フェーズ実施するか:**
> Claude Codeセッションを終了すると会話履歴は引き継がれない。
> `/rename` しておくことで、`/resume` でその時点の文脈を呼び戻して作業を再開できる。
> CLAUDE.mdが「**何を**作っているか」を伝えるなら、
> セッション名は「**どこまで**作業したか」を伝えるブックマーク。

以下を試す:
```
/rename phase1-design-session
```

セッション終了 → `claude` 再起動 → `/resume` でセッション一覧から選択して再開する体験をする。
一覧に「phase1-design-session」が表示されることを確認する。

---

## ✅ Phase 1 完了チェック

- [ ] Plan Modeで設計書を作成できた
- [ ] CLAUDE.mdの設計決定ログが更新された
- [ ] `/resume` でセッションを再開できた
- [ ] `/memory` でメモリ確認・記録ができた

## Phase 1 コミット & プッシュ【手動・featureブランチ初体験】

```powershell
git checkout -b feature/phase1-design
git add CLAUDE.md
git commit -m "docs(phase1): add design decisions and taskr architecture plan"
git push origin feature/phase1-design
gh pr create `
  --title "docs(phase1): add design decisions" `
  --body "Plan Modeで決定した設計内容をCLAUDE.mdに記録" `
  --base main
gh pr merge --merge
git checkout main
git pull origin main
```

> **PowerShellの改行:** PowerShellでコマンドを複数行に分ける場合はバッククォート `` ` `` を使う（macOSの `\` の代わり）。

セッション管理:
```
/rename phase1-design-complete
```

**→ セッション終了。新しいセッションでPhase 2を開始する**

---

---

# Phase 2: 初期実装 + git統合 + Skills 初体験

**このフェーズで習得すること:**
- バイブコーディングの基本リズム
- `/commit` スキル（コミットメッセージ自動生成）
- gh CLI基本操作

**セッション開始:** 新しいセッション。

> **Skillsとは:**
> `~\.claude\skills\<name>\SKILL.md` に定義した再利用可能なプロンプトテンプレート。
> `/skill-name` で呼び出せるショートカットコマンド。
> 組み込みスキル: `/commit`, `/commit-push-pr`, `/feature-dev`, `/revise-claude-md`

**フェーズ開始時にブランチを作成（手動）:**
```powershell
git checkout -b feature/phase2-core-impl
```

---

## タスク P2-1: プロジェクト構造の作成

```
taskrプロジェクトのディレクトリ構造を作成してください。
Windows環境（PowerShell）で実行してください。

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

以下を **順番に** 投げる:

```
# Step 1: データモデル
models.py に Task dataclass を実装してください。
フィールド: id(UUID), title(str), done(bool), created_at(datetime), tags(list[str])
JSON変換メソッド（to_dict / from_dict）も含めてください。
Windowsのパス区切り文字に注意してください。
```

```
# Step 2: ストレージ（Step 1確認後）
storage.py にJSON永続化を実装してください。
保存先: $env:USERPROFILE\.taskr\tasks.json（Windowsのホームディレクトリ）
pathlib.Path を使ってOSに依存しないパス処理にしてください。
CRUD操作: create, list_all, update, delete
```

```
# Step 3: CLIエントリポイント（Step 2確認後）
cli.py にclickを使ったCLIを実装してください。
コマンド: add, list, done, delete
まず add だけ実装して動作確認してから残りを追加してください。
```

```powershell
# 動作確認
uv run python -m taskr add "最初のタスク"
uv run python -m taskr list
```

## タスク P2-3: テストの作成

```
tests/test_storage.py に pytest のテストを書いてください。
tmp_path フィクスチャで一時ディレクトリを使い、
$env:USERPROFILE\.taskr\ の本番データに影響しないようにしてください。
カバー範囲: タスク追加・取得・更新・削除の4操作
```

```powershell
uv run pytest  # テストが通ることを確認
```

## タスク P2-4: /commit スキル初体験

```
/commit
```

→ Claudeが変更差分を分析して適切なコミットメッセージを提案してくれる。
→ 内容を確認して承認する。

## タスク P2-5: gh CLI でIssueを作成

```
gh CLIを使って以下のIssueをGitHubに作成してください:
タイトル: "Phase 3: Hooks設定を実施する"
ラベル: enhancement
本文: "PostToolUse Hookでruffとpytestを自動実行する設定を追加する"
```

---

## ✅ Phase 2 完了チェック

- [ ] `uv run python -m taskr add "テスト"` が動作する
- [ ] `uv run pytest` でテストがすべてパスする
- [ ] `/commit` スキルを使ってコミットできた

## Phase 2 コミット & プッシュ【/commit スキル + 手動push/PR】

```powershell
git add -u
```

```
/commit
```

```powershell
git push origin feature/phase2-core-impl
gh pr create `
  --title "feat(phase2): implement core taskr CLI" `
  --body "add/list/done/deleteコマンドとJSONストレージを実装。pytestテスト付き。" `
  --base main
gh pr merge --merge
git checkout main; git pull origin main
```

セッション管理:
```
/rename phase2-impl-complete
```

**→ セッション終了。新しいセッションでPhase 3を開始する**

---

---

# Phase 3: Hooks設定 + カスタムSkill作成

**このフェーズで習得すること:**
- PostToolUse / PreToolUse / SessionStart Hook の設定
- Windows向けのHookスクリプト（PowerShell）
- カスタムSkillの自作

**セッション開始:** 新しいセッション。

**フェーズ開始時にブランチを作成（手動）:**
```powershell
git checkout -b feature/phase3-hooks-skill
```

---

## タスク P3-1: PostToolUse Hook（自動フォーマット + テスト自動実行）

```
~\.claude\settings.json に PostToolUse フックを2つ追加してください。
Windows（PowerShell）環境向けのスクリプトで実装してください。

フック1: Write ツール使用後に ruff で自動フォーマット
  対象: *.py ファイルのみ
  コマンド: PowerShellでtaskrディレクトリに移動してuv run ruff format を実行

フック2: Bash ツール使用後に pytest を自動実行
  条件: tests\ ディレクトリが存在する場合のみ
  コマンド: PowerShellでtaskrディレクトリに移動してuv run pytest --tb=short を実行

設定後、Pythonファイルを少し変更してフックが動くか確認してください。
```

## タスク P3-2: PreToolUse Hook（安全ガード）

```
PreToolUse フックを追加してください。
Bash ツールで危険なコマンド（"Remove-Item -Recurse -Force" や "rd /s /q"）が
実行されようとした場合に警告を出して停止するPowerShellスクリプトを作成してください。

スクリプトは $env:USERPROFILE\study\workspace\learn_claude\.claude\hooks\safety_check.ps1
に保存してください。
```

## タスク P3-3: SessionStart Hook

```
SessionStart フックを設定してください。
セッション開始時に以下を表示するPowerShellスクリプトを作成してください:
- 現在の日時
- git status の出力
- 直近3件のコミットログ

スクリプトは $env:USERPROFILE\.claude\hooks\session_start.ps1 に保存してください。
```

> **Windowsでの通知（Notification Hook）**
> macOSの `osascript` の代わりに、PowerShellの `BurntToast` モジュールまたは
> `[System.Windows.Forms.MessageBox]` を使って通知を送れます。

## タスク P3-4: .claudeignore の整備

```
taskr\.claudeignore を確認・更新してください。
Windows特有のファイル（Thumbs.db, desktop.ini 等）も除外対象に含めてください。
```

## タスク P3-5: カスタムSkillの自作【Skills必須】

```
カスタムスキルを作成してください。

ファイル: $env:USERPROFILE\.claude\skills\taskr-status\SKILL.md
内容:
---
name: taskr-status
description: taskrプロジェクトの現在の実装状況とテストカバレッジを確認して報告する
allowed-tools: Read, Bash, Grep, Glob
---

以下を確認してレポートせよ:
1. uv run pytest --tb=short の結果
2. uv run ruff check . の結果
3. 実装済みのCLIコマンド一覧
4. CLAUDE.mdの「現在のフェーズ」を確認して次にやるべきことを提示

スキル作成後、/taskr-status で呼び出して動作確認してください。
```

## タスク P3-6: セッション終了の半自動化【/end-phase カスタムSkill】

毎フェーズ末に `/rename` の名前を手動で考えるのは手間です。
カスタムSkillを作って半自動化しましょう。

```
以下のカスタムSkillを作成してください。

ファイル: $env:USERPROFILE\.claude\skills\end-phase\SKILL.md
内容:
---
name: end-phase
description: フェーズ完了時のセッション管理を補助する。現在の作業内容から適切なセッション名を自動提案し、CLAUDE.mdの現在フェーズを更新する。
allowed-tools: Read, Edit
---

以下のステップで処理せよ:

1. CLAUDE.md を読んで「現在のフェーズ」と直近の作業内容を確認する
2. 作業内容から適切なセッション名（英語ケバブケース）を提案する
   例: phase3-hooks-skill-complete
3. 以下の形式でコピペしやすく出力する:

   📋 セッション終了コマンド（コピペして実行してください）:
   /rename <提案した名前>

4. CLAUDE.mdの「現在のフェーズ」を次のフェーズに更新してよいか確認する。
   OKなら自動で更新する。

スキル作成後、/end-phase を実行して動作確認してください。
```

> **このSkillの効果:**
> - セッション名を毎回考えなくて済む（Claudeが文脈から自動提案）
> - CLAUDE.mdの「現在のフェーズ」更新も忘れなくなる
> - **Phase 3以降はすべてのフェーズ末に `/end-phase` を使う**
>
> **注:** `/rename` はClaude Code内部コマンドのためSkillが直接実行できない。
> Skillが提案するコマンドをコピペして実行する形になる（それでも手動より大幅に楽）。

---

## ✅ Phase 3 完了チェック

- [ ] Pythonファイル保存時にruffが自動実行される
- [ ] Bashツール実行後にpytestが自動実行される
- [ ] `/taskr-status` コマンドが動作する
- [ ] `/end-phase` コマンドがセッション名を自動提案してくれる

## Phase 3 コミット & プッシュ【/commit + 手動push/PR（定着）】

```powershell
git add taskr\.claudeignore taskr\.claude\ CLAUDE.md
```

```
/commit
```

```powershell
git push origin feature/phase3-hooks-skill
gh pr create `
  --title "feat(phase3): add hooks and taskr-status custom skill" `
  --body "PostToolUse/PreToolUse/SessionStart フックを設定。カスタムSkill(/taskr-status)を自作。" `
  --base main
gh pr merge --merge
git checkout main; git pull origin main
```

セッション管理（`/end-phase` スキルを使う）:
```
/end-phase
```
→ Claudeが作業内容から適切なセッション名を提案してくれる。提案された `/rename` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 4を開始する**

---

---

# Phase 4: MCP統合 + Subagent 初体験

**このフェーズで習得すること:**
- `.mcp.json` の設定
- GitHub MCP によるIssue/PR管理
- Subagent（Task tool）の基本操作

**セッション開始:** 新しいセッション。

**フェーズ開始時にブランチを作成（手動）:**
```powershell
git checkout -b feature/phase4-mcp-subagent
```

---

## タスク P4-1: GitHub MCP のセットアップ

```
GitHub MCPをセットアップしてください。

$env:USERPROFILE\study\workspace\learn_claude\.mcp.json を作成して
gh mcp serve を設定してください。
設定後、以下を確認:
1. GitHub MCPでlearn_claudeリポジトリのIssue一覧を取得
2. Phase 3で作成したIssueをMCP経由でクローズ
```

## タスク P4-2: Subagent 初体験【Subagent必須】

```
Subagent（Task tool）を使って調査タスクを委託してください。

依頼内容:
「clickライブラリを使ったCLIのテスト方法（pytestとの組み合わせ）を
 codebaseを調査した上でベストプラクティスを報告してください」

Explore型のSubagentを使い、結果をメインセッションに返してもらってください。
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

## タスク P4-4: /commit-push-pr スキル初体験

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

## Phase 4 コミット & プッシュ【/commit-push-pr スキル初体験】

```
/commit-push-pr
```

```powershell
gh pr merge --merge
git checkout main; git pull origin main
```

セッション管理（`/end-phase` スキルを使う）:
```
/end-phase
```
→ Claudeが作業内容から適切なセッション名を提案してくれる。提案された `/rename` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 5を開始する**

---

---

# Phase 5: Permission設定

**このフェーズで習得すること:**
- allow/denyルールの設計と実装（Windows環境向け）
- グローバル設定とプロジェクト設定の優先順位

**セッション開始:** 新しいセッション。

**フェーズ開始時にブランチを作成（手動）:**
```powershell
git checkout -b feature/phase5-permissions
```

---

## タスク P5-1: Permission設定の理解と実装

```
~\.claude\settings.json の Permission 設定を実装してください。
Windows（PowerShell）環境向けのコマンドパターンで設定してください。

allow（自動承認するコマンド）:
- "Bash(git *)"
- "Bash(uv run pytest *)"
- "Bash(uv run ruff *)"
- "Bash(gh *)"
- "Write(*.py)"
- "Write(*.md)"
- "Write(*.toml)"

deny（禁止するコマンド）:
- "Bash(Remove-Item -Recurse *)"
- "Bash(rd /s *)"

各ルールの意味と設定理由を説明してから実装してください。
```

## タスク P5-2: プロジェクト固有設定

```
taskr\.claude\settings.json（プロジェクトレベル設定）を作成してください。
グローバル設定との優先順位を説明し、
このプロジェクト固有に必要な設定を検討して追加してください。
```

## タスク P5-3: 動作確認実験

```
以下を試して、Permissionルールが正しく機能するか確認してください:
1. Remove-Item -Recurse -Force $env:TEMP\test_permission を試してdenyされることを確認
2. uv run pytest が自動承認されることを確認
3. 設定の効果をCLAUDE.mdの設計決定ログに記録する
```

---

## ✅ Phase 5 完了チェック

- [ ] 危険な削除コマンドがdenyされた
- [ ] `uv run pytest` が自動承認された
- [ ] グローバルとプロジェクト設定の優先順位を理解した

## Phase 5 コミット & プッシュ【/commit-push-pr（定着）】

```
/commit-push-pr
```

```powershell
gh pr merge --merge
git checkout main; git pull origin main
```

セッション管理（`/end-phase` スキルを使う）:
```
/end-phase
```
→ Claudeが作業内容から適切なセッション名を提案してくれる。提案された `/rename` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 6を開始する**

---

---

# Phase 6: Plan Mode本格活用 + Subagent応用

**このフェーズで習得すること:**
- `claude --plan` で大機能を設計してから実装するフロー
- Subagentを並列で動かして設計を補強する
- 設計判断の文書化

**セッション開始:** 新しいセッション。

---

## タスク P6-1: Plan Modeで大機能を設計

```powershell
claude --plan
```

```
taskrに以下の機能を追加する計画を立ててください（コードは書かない）:
- 優先度システム (high / medium / low)
- 期限設定 (due date、YYYY-MM-DD形式)

設計書に含めること:
- データモデルの変更（Taskクラスへの追加フィールド）
- 既存データとの後方互換性戦略
- CLI引数の変更
- テストの追加計画
- 実装の順序
Windows環境でのパス処理についても考慮してください。
```

## タスク P6-2: Subagentによる並列調査【Subagent応用】

```
Plan Modeの設計判断を補強するため、Subagentを2つ並列で動かしてください。

Subagent 1（バックグラウンド）:
「CLIツールにおける優先度実装のベストプラクティスを調査してください」

Subagent 2（バックグラウンド）:
「Pythonのdatetime/date型の扱い方とJSONシリアライズ方法を調査してください」

2つのSubagentの調査結果をまとめて、設計書を最終化してください。
```

## タスク P6-3: 実装（/fast モード使用）

```
/fast
```

```
設計書に従って優先度と期限機能を実装してください。
実装順序: models.py → storage.py → cli.py → テスト追加
各ステップで uv run pytest を実行して既存テストが壊れていないか確認してください。
```

## タスク P6-4: 設計判断をCLAUDE.mdに記録

```
CLAUDE.mdの「設計決定ログ」に以下を追記してください:
- 優先度・期限機能の設計方針とその理由
- 後方互換性の解決方法
- 設計書から変更した点と理由
```

---

## ✅ Phase 6 完了チェック

- [ ] Plan Modeで設計書を作成できた
- [ ] Subagentを2つ並列で動かせた
- [ ] 優先度・期限機能が動作する
- [ ] CLAUDE.mdに設計判断が記録された

## Phase 6 コミット & プッシュ【プロンプト一発でブランチ〜PR（フル自動化）】

```
feature/phase6-priority-duedate ブランチを作成して、
今フェーズの実装内容をコミット・プッシュして、PRを作成してください。
PRのタイトルとdescriptionは変更内容から自動生成してください。
PowerShell環境でgitコマンドを実行してください。
```

```powershell
gh pr merge --merge
git checkout main; git pull origin main
```

セッション管理（`/end-phase` スキルを使う）:
```
/end-phase
```
→ Claudeが作業内容から適切なセッション名を提案してくれる。提案された `/rename` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 7を開始する**

---

---

# Phase 7: Agent Teams 基礎

**このフェーズで習得すること:**
- Agent Teamsの起動と仕組み
- 2エージェント並列実行の体験
- Worktreeを使った隔離実験
- バックグラウンドTask実行

**前提:** `$env:CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` → `1` が返ること

> Agent Teamsの動作にはWindowsの場合 **WSL2** または **Git Bash** が必要な場合があります。
> 動作しない場合はClaude Codeに確認してください。

**セッション開始:** 新しいセッション。

---

## タスク P7-1: Agent Teamsの仕組みを理解

```
Agent Teams機能について説明してください:
1. どのような仕組みで動くか
2. Subagentとの違い
3. Worktree隔離とは何か
4. Windows環境での注意点があれば教えてください
5. このtaskrプロジェクトでAgent Teamsを使うのに適したシナリオを3つ提案してください
```

## タスク P7-2: 2エージェント並列実行体験【Agent Teams必須】

```
Agent Teamsを使って以下を並列実行してください:

エージェント1（テスト担当）:
- test_models.py の作成（Taskクラスの全メソッドをテスト）
- test_storage.py の更新（priority, due_dateのテストを追加）

エージェント2（ドキュメント担当）:
- README.md の更新（全コマンドの使い方を追記）
- CHANGELOG.md の作成（バージョン履歴の開始）

2エージェントが完了したら結果を統合して、テストが通ることを確認してください。
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

## タスク P7-4: バックグラウンドTask実行

```
Task toolでバックグラウンド処理を実行してください:

バックグラウンドで実行:
- uv run pytest --cov=taskr でカバレッジ計測
- uv run ruff check . でリント全体チェック

メインスレッドでは:
- バックグラウンド完了を待ちながら、CLAUDE.mdにPhase 7の学習記録を追記する

バックグラウンド処理が完了したら結果を確認してください。
```

---

## ✅ Phase 7 完了チェック

- [ ] 2エージェントが並列で異なるファイルを編集した
- [ ] Worktreeで実験ブランチを作成・利用できた
- [ ] バックグラウンドTask実行を体験した

## Phase 7 コミット & プッシュ【Agent Teams + フル自動GitHub Flow】

```
Agent Teamsでの作業が完了しました。
feature/phase7-agent-teams ブランチを作成して、
テストとドキュメントの変更をまとめてコミット・プッシュ・PR作成してください。
PR本文には「Agent Teamsで並列実装した内容の概要」を自動で記述してください。
PowerShell環境でgitコマンドを実行してください。
```

```powershell
gh pr merge --merge
git checkout main; git pull origin main
```

セッション管理（`/end-phase` スキルを使う）:
```
/end-phase
```
→ Claudeが作業内容から適切なセッション名を提案してくれる。提案された `/rename` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 8を開始する**

---

---

# Phase 8: Agent Teams 応用

**このフェーズで習得すること:**
- `/feature-dev` ワークフロー
- 複数エージェントの協調とコンフリクト解消
- GitHub Actions CI/CD の設定（Windows対応含む）

**セッション開始:** 新しいセッション。

---

## タスク P8-1: /feature-dev でレポート機能実装【Agent Teams本格活用】

```
/feature-dev
```

`/feature-dev` がない場合:

```
Agent Teamsを使って「taskr report」コマンドを実装してください。

要件:
- taskr report で統計情報を表示
- 完了/未完了タスク数、優先度別集計、タグ別集計

エージェント分担:
- エージェントA: storage.pyに集計ロジックを追加
- エージェントB: cli.pyにreportコマンドを追加
- エージェントC: テストとREADME更新

各エージェントの作業完了後に統合してください。
```

## タスク P8-2: コンフリクト解消練習

```
Agent Teamsを使って、意図的にコンフリクトを発生させ、解消を練習してください:

エージェント1: cli.py に taskr export コマンドを追加（JSON/CSV出力）
エージェント2: cli.py に taskr import コマンドを追加（JSONインポート）

コンフリクトの解消プロセスを実践してください。
```

## タスク P8-3: GitHub Actions CI設定（Windows対応）

```
.github/workflows/ci.yml を作成してください:

トリガー: push, pull_request（mainブランチ）
jobs:
- test: ubuntu-latest と windows-latest でpytest実行（クロスプラットフォーム）
- lint: uv run ruff check
- coverage: カバレッジ80%以上を要件に

Windows環境でのパス処理テストも含めてください。
ワークフローファイル作成後にプッシュして、
GitHub MCPでActions実行状況を確認してください。
```

---

## ✅ Phase 8 完了チェック

- [ ] `/feature-dev` ワークフローを体験した
- [ ] コンフリクト解消を実践した
- [ ] GitHub ActionsのCIがubuntu+windowsで動作している

## Phase 8 コミット & プッシュ【GitHub MCP + Agent Teams完全統合】

```
Agent Teamsで実装したすべての変更を
feature/phase8-advanced ブランチにまとめてください。
その後:
1. /commit-push-pr でコミット・プッシュ・PR作成
2. GitHub MCPを使ってPRに "enhancement" ラベルを付与
3. PR descriptionにAgent Teamsの作業分担を自動で記録

これをすべてClaude Codeにやってもらう（人間は最終確認のみ）。
```

```powershell
gh pr merge --merge
git checkout main; git pull origin main
```

セッション管理（`/end-phase` スキルを使う）:
```
/end-phase
```
→ Claudeが作業内容から適切なセッション名を提案してくれる。提案された `/rename` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 9を開始する**

---

---

# Phase 9: 総仕上げ【全機能統合 + v1.0.0リリース】

**このフェーズで習得すること:**
- `/revise-claude-md` による最終整備
- `keybindings.json` のカスタマイズ
- GitHub Releasesへの公式リリース
- 学習の振り返り

**セッション開始:** 新しいセッション。

---

## タスク P9-1: CLAUDE.md の最終整備

```
/revise-claude-md
```

```
CLAUDE.mdを完全版に整備してください:
1. プロジェクト概要と目的
2. Windows環境のセットアップ手順（環境構築の要約）
3. 全コマンド使い方ガイド（PowerShell用のコマンド例）
4. アーキテクチャ説明
5. 設計決定ログ（全フェーズ分）
6. Claude Codeを使った開発のコツ（Windows環境での注意点も含む）

次のClaudeセッションで即座に開発を再開できるレベルに整えること。
```

## タスク P9-2: keybindings.json のカスタマイズ

```
~\.claude\keybindings.json を作成してください。
keybindings.jsonで設定できる内容と書き方を説明してから、
よく使う操作のショートカットを設定してください。
```

## タスク P9-3: インタラクティブモードの実装（Agent Teams総動員）

```
Agent Teamsを使って taskr -i（インタラクティブモード）を実装してください。

Subagent（バックグラウンド）:
TUIライブラリの選定調査（questionary, prompt_toolkit, rich を比較して推奨を報告）
Windows Terminalでの動作確認も含めること

Agent Teams:
- エージェント1: TUIライブラリ導入と基本UI実装
- エージェント2: キーバインドとナビゲーション
- エージェント3: 既存CLIとの統合とテスト

uv add でライブラリを追加してから実装してください。
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

/commit-push-pr スキルで最終PRを作成してください。
```

## タスク P9-5: 学習振り返り

```
このカリキュラムを通じた学びをCLAUDE.mdに追記してください。

「学習まとめ」セクション:
1. 各フェーズで習得したClaude Code機能
2. Skills / Subagent / Agent Teams それぞれの使いどころと実感
3. バイブコーディングで効果的だったプロンプトパターン
4. 仕事で活用できるフロー（プランニング・コミュニケーション保存）
5. GitHub Flow自動化の段階的変化の振り返り
6. Windows環境で特有のハマりポイントと解決方法

GitHub Issueで今後のロードマップを5件作成してください。
```

---

## ✅ Phase 9 完了チェック

- [ ] `uv run python -m taskr --version` が `1.0.0` を返す
- [ ] GitHub Releasesに v1.0.0 が公開されている
- [ ] CLAUDE.mdの学習まとめが完成している
- [ ] keybindings.jsonが設定済み

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

PowerShell環境でgitコマンドを実行してください。
人間が行うのは最終確認のみ。
```

セッション管理（`/end-phase` スキルを使う）:
```
/end-phase
```
→ Claudeが作業内容から適切なセッション名を提案してくれる。提案された `/rename` コマンドをコピペして実行する。

---

---

# 🎉 カリキュラム完了

おめでとうございます。以下の機能を習得しました:

| 習得項目 | 確認方法 |
|---------|---------|
| **Skills** | `/commit`, `/commit-push-pr`, `/revise-claude-md`, カスタム `/taskr-status` |
| **Subagent** | Phase 4, 6, 7, 9 で並列調査・実装を委託した |
| **Agent Teams** | Phase 7〜9 で複数エージェントが協調して作業した |
| **Plan Mode** | Phase 1, 6 で設計書を作成してから実装した |
| **コミュニケーション保存** | CLAUDE.md + /memory + /rename で全フェーズを引き継いだ |
| **GitHub Flow** | 手動操作 → Skills自動化 → 完全自動化の変遷を体験した |

**taskr v1.0.0 のリポジトリを公開してカリキュラム完了！**

---

## macOS版との主な差異

| 項目 | macOS版 | Windows版 |
|-----|--------|---------|
| ターミナル | zsh | PowerShell 7 |
| 改行継続 | `\` | バッククォート `` ` `` |
| 環境変数設定 | `~/.zshrc` | `$PROFILE`（PowerShell プロファイル） |
| 環境変数参照 | `$VAR` | `$env:VAR` |
| ホームディレクトリ | `~` | `$env:USERPROFILE`（または `~`） |
| uv インストール | `curl -LsSf ... \| sh` | `irm ... \| iex`（PowerShell） |
| パッケージ管理 | Homebrew (`brew`) | winget |
| 通知 | `osascript` | PowerShell / BurntToast |
| 危険コマンド | `rm -rf` | `Remove-Item -Recurse` / `rd /s` |
| CI マトリックス | ubuntu-latest | ubuntu-latest + windows-latest |
