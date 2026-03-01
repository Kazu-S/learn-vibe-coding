# Codex CLI バイブコーディング学習カリキュラム（macOS版）

## このドキュメントの使い方

このファイル自体がカリキュラムです。上から順番に読み進め、各タスクのプロンプトをCodex CLIに投げながら学習を進めてください。

**学習スタイル:** バイブコーディング（AIと対話しながらコードを作り上げる）
**制作物:** Python CLIタスク管理ツール「taskr」
**期間目安:** 3週間（週5〜10時間）

---

## 変更履歴

| 日付 | 変更内容 |
|------|---------|
| 2026-02-28 | 初版作成（Phase 0〜9、macOS版） |

---

## ⚠️ コンテキスト管理ルール（重要）

**各フェーズは独立したCodexセッションで実施すること。**

フェーズ完了の手順:
1. フェーズ内のタスクをすべて完了
2. featureブランチを commit → push → Pull Request作成（フェーズにより手動/カスタムコマンド）
3. `/fork` で現在のセッションを分岐・保存（作業状態の記録）
4. Codex CLI セッションを終了
5. 次のフェーズは **新しいセッション** で開始（CodexはAGENTS.mdを読んでコンテキストを把握する）

**なぜセッションを分けるか:**
- コンテキストウィンドウの使用量を管理するため
- フェーズの境界を明確にするため
- AGENTS.mdが「セッション間の引き継ぎノート」として機能するため

**セッション再開方法:**
```bash
codex resume --last   # 直前のセッションを再開
codex resume          # セッション一覧から選択
# または Codex CLI 内で
/resume
```

---

## GitHub Flow の学習ステップ（自動化の段階的移行）

git/GitHub操作はフェーズを追うごとに自動化されていきます：

| フェーズ | GitHub Flow | 操作 |
|---------|------------|------|
| Phase 1 | 手動フルフロー | `git checkout -b` → `git add` → `git commit` → `git push` → `gh pr create` |
| Phase 2〜3 | /commit 初体験 | 手動branch + **`/commit`**（メッセージ自動生成）+ 手動push/PR |
| Phase 4〜5 | /commit-push-pr | 手動branch + **`/commit-push-pr`**（add+commit+push+PR一括） |
| Phase 6〜 | 完全自動 | プロンプト一発でbranch〜PRまで全自動 |
| Phase 7〜9 | codex exec統合 | 実装完了後に自動PR・自動マージ |

---

## フェーズ一覧

| フェーズ | 内容 | 主な習得機能 | 目安時間 |
|---------|------|------------|---------|
| **事前準備** | VS Code + Codex CLI インストール | - | 30分 |
| **Phase 0** | Python環境構築（Codex CLI半自動） | uv, gh CLI, GitHub連携 | 1〜2時間 |
| **Phase 1** | 基本対話・設計・保存 | Suggest Mode, AGENTS.md, セッション管理 | 2時間 |
| **Phase 2** | 初期実装 + git統合 + カスタムコマンド | /commit, バイブコーディング | 3時間 |
| **Phase 3** | notify設定 + カスタムコマンド自作 | notify設定, カスタムコマンド | 2〜3時間 |
| **Phase 4** | MCP + Subagent | GitHub MCP, subagent機能 | 2〜3時間 |
| **Phase 5** | 承認モード設定 | Suggest/Auto-Edit/Full-Auto | 1〜2時間 |
| **Phase 6** | Suggest Mode本格活用 + Subagent応用 | /status, 並列Subagent | 2〜3時間 |
| **Phase 7** | Subagent Teams基礎 | 並列subagent, max_depth | 3時間 |
| **Phase 8** | Subagent Teams応用 + codex exec | /commit-push-pr, CI/CD自動化 | 3〜4時間 |
| **Phase 9** | 総仕上げ + v1.0.0リリース | config.toml最終化, Release | 3〜4時間 |

---

## 必須習得項目

このカリキュラムを完了することで、以下5つを確実に使いこなせる状態になります：

| 項目 | 習得目標 |
|-----|---------|
| **カスタムコマンド** | `/commit`, `/commit-push-pr`, `/revise-agents-md` を使いこなし、カスタムコマンドを自作できる |
| **Subagent** | subagent機能で独立エージェントを起動し、調査・実装を並列委譲できる |
| **codex exec** | 非インタラクティブモードでCI/CDスクリプトとしてCodexを活用できる |
| **Suggest Mode** | `/status` + Suggest Modeで設計し、承認してから実装するフローを習慣化できる |
| **コミュニケーション保存** | AGENTS.md + /fork + セッション管理の3層でCodex対話を引き継げる |

---

---

# 🔧 事前準備（手動インストール・Codex CLI不使用）

**このセクションのみ人間が手動で実行します。** 完了後、以降はすべてCodex CLIを使って進めます。

---

## Step 1: VS Code のインストール

1. ブラウザで `https://code.visualstudio.com/` を開く
2. **Download for Mac** をクリック（Apple Silicon / Intel を選択）
3. ダウンロードした `.zip` を展開し、`Visual Studio Code.app` を `/Applications` フォルダへ移動
4. VS Codeを起動して動作確認
5. コマンドパレット（`Cmd+Shift+P`）を開き、「**Shell Command: Install 'code' command in PATH**」を実行
   → これによりターミナルから `code` コマンドが使えるようになる

```bash
code --version  # バージョンが表示されれば成功
```

---

## Step 2: Xcode Command Line Tools のインストール（git確保）

```bash
xcode-select --version
```

コマンドが見つからない場合:
```bash
xcode-select --install
```

ダイアログが表示されたら「インストール」をクリック。完了後:

```bash
git --version  # git version 2.x.x が表示されれば成功
```

---

## Step 3: OpenAI アカウントの作成

1. `https://platform.openai.com/` を開いてアカウントを作成
2. Codex CLIが利用できるプランに加入（ChatGPT Plus / Pro いずれか）
3. APIキーを発行: Settings → API Keys → Create new secret key
4. 環境変数に設定:

```bash
echo 'export OPENAI_API_KEY="sk-..."' >> ~/.zshrc
source ~/.zshrc
echo $OPENAI_API_KEY  # 値が表示されれば成功
```

---

## Step 4: Node.js のインストール（Codex CLI に必要）

```bash
node --version  # v18以上であればOK
```

インストールされていない場合: `https://nodejs.org/` から **LTS版** をダウンロードしてインストール

---

## Step 5: Codex CLI のインストール

方法A（npm）:
```bash
npm install -g @openai/codex
codex --version   # バージョンが表示されれば成功
```

方法B（Homebrew Cask）:
```bash
brew install --cask codex
codex --version
```

ログイン確認:
```bash
codex
# 起動後、/status で接続状態とAPIキーを確認
```

---

## Step 6: VS Code の拡張機能をインストール（任意）

Codex CLIはターミナルツールですが、VS Codeと組み合わせると便利です:

1. VS Codeを開く
2. 拡張機能検索で「**GitHub Copilot**」や「**REST Client**」などを必要に応じてインストール
3. Codex CLIはVS Code統合ターミナル内で動作します

> **補足:** Codex CLIはVS Code拡張としては提供されておらず、ターミナルで `codex` コマンドとして動作します。VS Codeの統合ターミナル（`Ctrl+\``）から起動するのが便利です。

---

## ✅ 事前準備 完了チェック

- [ ] `code --version` が動作する
- [ ] `git --version` が動作する
- [ ] `codex --version` が動作する
- [ ] `echo $OPENAI_API_KEY` でAPIキーが表示される
- [ ] `codex` 起動後に `/status` でAPI接続済みと表示される

**→ 以降はすべてCodex CLIを使って進めます**

---

---

# Phase 0: Python環境構築【Codex CLI 半自動セットアップ】

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- Codex CLIによる環境診断・半自動セットアップの体験
- uv（Python管理ツール）による仮想環境構築
- GitHubリポジトリの初期化

**セッション開始前に（手動）:** 学習作業を行うディレクトリを自分で決めて作成し、そこへ移動してから `codex` を起動する。

```bash
# 例: 好きな場所に作業ディレクトリを作成
mkdir -p ~/your/working/directory
cd ~/your/working/directory
codex   # このディレクトリで起動することが重要
```

> 以降の手順はすべて「codex を起動したディレクトリ」を基準に動作します。

> **Agent活用の原則（このフェーズから意識する）**
> メインセッションは「指示を出す」ことに専念し、実行はCodex CLIに任せる。
> コマンドは自分で打たず、Codex CLIの承認ダイアログで「承認」するだけ。

---

## ツール方針（企業利用・ライセンス安全基準）

| ツール | ライセンス | 商用利用 | 役割 |
|-------|----------|---------|-----|
| **uv** | MIT / Apache 2.0 | ✅ 制限なし | Python管理 + 仮想環境 + パッケージ管理 |
| **venv** | PSF（Python標準添付） | ✅ 制限なし | フォールバック用 |
| **gh CLI** | MIT | ✅ 制限なし | GitHub操作 |
| ~~conda / Anaconda~~ | 商用有償 | ❌ 大企業は要契約 | **使用禁止** |

---

## タスク P0-1: 環境診断をCodex CLIに依頼

Codex CLIに以下のプロンプトを投げる:

```
これからPython開発環境をゼロから構築します。
クリーンなmacOSを想定して、必要なツールを順番に診断してください。

チェック項目:
1. uv --version（~/.local/bin/uv, ~/.cargo/bin/uv を確認）
2. python3 --version
3. gh --version（GitHub CLI）

結果をリストアップして、不足しているものとインストール方法を教えてください。
インストールコマンドは私の確認を取ってから1つずつ実行してください。
企業利用でライセンス問題のないツールのみ使用すること。
```

## タスク P0-2: uv のインストール

Codex CLIが以下を提案するので承認するだけ:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.zshrc   # PATHを反映
uv --version      # 確認
```

## タスク P0-3: gh CLI のインストール

```bash
# Homebrewがある場合
brew install gh

# ない場合はCodex CLIが適切な方法を案内してくれる
gh auth login   # ブラウザでGitHub認証
gh auth status  # 認証済みと表示されることを確認
```

## タスク P0-4: Python + 仮想環境のセットアップ

```
uvを使ってtaskrプロジェクトのPython環境をセットアップしてください。

順番に実行してください（各ステップ前に確認を取ること）:
1. uv python install 3.12
2. mkdir taskr
3. cd taskr
4. uv init --python 3.12
5. uv venv
6. uv add click
7. uv add --dev pytest ruff pytest-cov
8. uv run python --version で動作確認
9. pyproject.tomlの内容を表示して確認

各コマンドは確認を取ってから実行してください。
```

## タスク P0-5: GitHubリポジトリ作成 + AGENTS.md 初期化

```
以下を順番に実施してください:

1. gh repo create learn_claude --private でGitHubリポジトリを作成
2. git init（カレントディレクトリで実行）
3. git remote add origin [リポジトリURL]
4. ~/.codex/config.toml を以下の内容で作成:

```toml
model = "o4-mini"
approval_policy = "suggest"
personality = "pragmatic"
```

5. AGENTS.md を以下の内容で作成:

```markdown
# taskr プロジェクト

## 概要
Codex CLI バイブコーディング学習プロジェクト。
Python CLIタスク管理ツール「taskr」を構築しながらCodex CLIを習得する。

## Python環境
- Pythonバージョン: 3.12
- パッケージ管理: uv
- 仮想環境: taskr/.venv/
- 実行方法: uv run <command>

## テストコマンド
- テスト実行: uv run pytest
- リント: uv run ruff check .
- フォーマット: uv run ruff format .

## 現在のフェーズ
Phase 0 完了 → Phase 1 へ

## 設計決定ログ
（各フェーズで追記していく）
```

各ステップの結果を報告しながら進めてください。
```

## タスク P0-6: .gitignore の作成

```
.gitignore を作成してください。

.gitignore に含めるもの:
- .venv/
- __pycache__/
- *.pyc
- .pytest_cache/
- *.egg-info/
- dist/
- .DS_Store
- .codex/

なぜこれらを除外するのかも説明してください。
```

---

## ✅ Phase 0 完了チェック

- [ ] `uv run python --version` → Python 3.12.x
- [ ] `uv run pytest --version` → pytestバージョンが表示される
- [ ] `gh auth status` → 認証済みと表示される
- [ ] `AGENTS.md` が作成済み
- [ ] GitHubリポジトリが作成済み
- [ ] `~/.codex/config.toml` が作成済み
- [ ] `/status` でモデル・承認ポリシーが表示される

## Phase 0 コミット & プッシュ【手動操作】

Phase 0はmainブランチへ直接コミット（初期セットアップのため）。コマンドの意味を理解しながら一つずつ手動で実行する。

```bash
git add AGENTS.md .gitignore taskr/pyproject.toml
git commit -m "feat(phase0): initial project setup with Python 3.12 via uv"
git push -u origin main
```

セッション管理:
```
/fork
```

> **注:** `/fork` はCodex CLIで現在のセッション状態を分岐・保存するコマンドです。フェーズ終了時に実行することで、`codex resume` で後から参照できます。

**→ ここでCodexセッションを終了して、新しいセッションでPhase 1を開始する**

---

---

# Phase 1: 基本対話・設計・コミュニケーション保存

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- スラッシュコマンド（/help, /model, /status, /permissions, /resume）
- Suggest Mode初体験（設計フェーズでの活用）
- AGENTS.mdへの保存習慣（コミュニケーション保存）
- セッション管理と再開

**セッション開始:** 新しいターミナルで `codex` を起動。AGENTS.mdが自動読込されることを確認する。

---

## タスク P1-1: スラッシュコマンドの探索

以下を順番に試す:

```
/help
```
→ コマンド一覧を確認する

```
/status
```
→ 現在のモデル・承認モード・トークン使用量を確認する

```
/model
```
→ o4-mini から o3 に切り替えてから o4-mini に戻す

```
/permissions
```
→ 承認モードを確認する（Suggest / Auto-Edit / Full-Auto の3段階を確認）

- `Shift+Enter` で複数行入力を試す

## タスク P1-2: Suggest Mode 初体験

> **Suggest Modeとは:**
> Codex CLIにおける「提案のみ」の承認モード。Claude CodeのPlan Modeに相当する設計段階での活用法。
> `/permissions` でSuggestモードを選択すると、Codexはファイル変更やコマンド実行を提案するが、
> 人間が明示的に承認するまでは何も実行しない。
>
> **Plan Mode（Claude Code）との対応:**
> - Claude Code: `claude --plan` または Shift+Tab×2 で起動
> - Codex CLI: `/permissions` → Suggest を選択し、`/status` で設計内容を確認・整理する

```
/permissions
```

「Suggest」を選択してから以下を投げる:

```
taskrの基本設計を計画してください（コードは書かない）。

検討事項:
- データモデル（Taskの属性: id, title, done, created_at, tags）
- 保存形式（JSONを選定済み。その理由を整理する）
- CLIコマンド一覧（add, list, done, delete）
- ファイル構成（src layout vs flat layout）

設計書としてまとめ、私が確認してから次に進みます。
```

→ 設計書を確認して承認（Approve）

```
/status
```

→ セッション状態・トークン使用量・現在の承認モードを確認する

## タスク P1-3: コミュニケーション保存（AGENTS.md更新）

```
Suggest Modeで決定した設計内容をAGENTS.mdに記録してください。

「設計決定ログ」セクションに以下を追記:
- 決定したデータモデル
- 保存形式の選定理由
- CLIコマンド一覧
- ファイル構成の決定

その後、このセッションで /fork コマンドを実行して作業状態を保存してください。
```

> **コミュニケーション保存の3層構造**
> ```
> Layer 1: AGENTS.md    ← 長期保存（プロジェクト全体の知識・方針）
> Layer 2: /fork        ← 中期保存（セッション状態の分岐・保存）
> Layer 3: /resume      ← セッション管理（保存済みセッションの一覧・再開）
> ```

## タスク P1-4: /diff コマンドの確認

```
/diff
```

→ 現在のGit差分を確認する。AGENTS.mdの変更が表示されることを確認する。

```
/mention AGENTS.md
```

→ ファイルをコンテキストに明示的に追加する操作を試す。

## タスク P1-5: セッション管理の練習（/resume を理解する）

> **`/resume` とは:**
> 保存済みセッションを一覧表示して再開するコマンド。
> `codex resume --last` でターミナルから直前のセッションを再開できる。
> AGENTS.mdが「**何を**作っているか」を伝えるなら、
> セッション履歴は「**どこまで**作業したか」を伝えるブックマーク。

以下を試す:
```
/fork
```

セッション終了 → `codex` 再起動 → `/resume` でセッション一覧から選択して再開する体験をする。
保存したセッションが一覧に表示されることを確認する。

---

## ✅ Phase 1 完了チェック

- [ ] Suggest Modeで設計書を作成できた
- [ ] AGENTS.mdの設計決定ログが更新された
- [ ] `/resume` でセッションを再開できた
- [ ] `/status` でトークン使用量を確認できた
- [ ] `/diff` でGit差分を確認できた

## Phase 1 コミット & プッシュ【手動・featureブランチ初体験】

Phase 1からfeatureブランチを使うGitHub Flowを開始。**すべて手動で**コマンドを打つ（仕組みを体に覚えるため）。

```bash
# フェーズ冒頭でブランチを作成しておく（次フェーズから冒頭で実施する）
git checkout -b feature/phase1-design

# 作業完了後
git add AGENTS.md
git commit -m "docs(phase1): add design decisions and taskr architecture plan"
git push -u origin feature/phase1-design

# Pull Requestを手動で作成
gh pr create \
  --title "docs(phase1): add design decisions" \
  --body "Suggest Modeで決定した設計内容をAGENTS.mdに記録" \
  --base main

# PRをマージ
gh pr merge --merge
git checkout main
git pull origin main
```

セッション管理:
```
/fork
```

**→ セッション終了。新しいセッションでPhase 2を開始する**

---

---

# Phase 2: 初期実装 + git統合 + カスタムコマンド初体験

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- バイブコーディングの基本リズム（対話 → 実行 → 確認 → 修正）
- `/commit` カスタムコマンド（コミットメッセージ自動生成）
- gh CLI基本操作

**セッション開始:** 新しいセッション。Codex CLIがAGENTS.mdを読んでコンテキストを把握する。

> **カスタムコマンドとは:**
> `~/.codex/<name>.md` に定義した再利用可能なプロンプトテンプレート。
> `/name` で呼び出せるショートカットコマンド。
> Claude CodeのSkillsに相当する機能。

---

**フェーズ開始時にブランチを作成（手動）:**
```bash
git checkout -b feature/phase2-core-impl
```

## タスク P2-1: プロジェクト構造の作成

```
taskrプロジェクトのディレクトリ構造を作成してください。

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
```

```
# Step 2: ストレージ（Step 1確認後）
storage.py にJSON永続化を実装してください。
保存先: ~/.taskr/tasks.json
CRUD操作: create, list_all, update, delete
```

```
# Step 3: CLIエントリポイント（Step 2確認後）
cli.py にclickを使ったCLIを実装してください。
コマンド: add, list, done, delete
まず add だけ実装して動作確認してから残りを追加してください。
```

```bash
# 動作確認
uv run python -m taskr add "最初のタスク"
uv run python -m taskr list
```

## タスク P2-3: テストの作成

```
tests/test_storage.py に pytest のテストを書いてください。
tmp_path フィクスチャで一時ディレクトリを使い、本番データ（~/.taskr/）に影響しないようにしてください。
カバー範囲: タスク追加・取得・更新・削除の4操作
```

```bash
uv run pytest  # テストが通ることを確認
```

## タスク P2-4: /commit カスタムコマンド初体験

カスタムコマンドを作成して使ってみる:

```bash
# カスタムコマンドファイルを作成
mkdir -p ~/.codex
cat > ~/.codex/commit.md << 'EOF'
staged な変更に対して、Conventional Commits形式のコミットメッセージを生成してコミットしてください。
フォーマット: <type>(<scope>): <description>
type の候補: feat, fix, docs, test, refactor, chore
コミット前に生成したメッセージを提示して確認を取ること。
EOF
```

```
/commit
```

→ Codexが変更差分を分析して適切なコミットメッセージを提案してくれる。
→ 内容を確認して承認（またはメッセージを修正）する。
→ **手動でメッセージを考えなくていい体験**を味わう。

## タスク P2-5: gh CLI でIssueを作成

```
gh CLIを使って以下のIssueをGitHubに作成してください:
タイトル: "Phase 3: notify設定を実施する"
ラベル: enhancement
本文: "config.tomlのnotify設定でタスク完了時に通知音を鳴らす設定を追加する"
```

---

## ✅ Phase 2 完了チェック

- [ ] `uv run python -m taskr add "テスト"` が動作する
- [ ] `uv run pytest` でテストがすべてパスする
- [ ] `/commit` カスタムコマンドを使ってコミットできた

## Phase 2 コミット & プッシュ【/commit カスタムコマンド + 手動push/PR】

コミットはカスタムコマンドに委譲し、push・PRはまだ手動：

```bash
git add -u   # 手動でステージング
```

```
/commit      # ← コミットメッセージ自動生成。承認するだけ。
```

```bash
# push と PR作成は手動
git push -u origin feature/phase2-core-impl
gh pr create \
  --title "feat(phase2): implement core taskr CLI" \
  --body "add/list/done/deleteコマンドとJSONストレージを実装。pytestテスト付き。" \
  --base main
gh pr merge --merge
git checkout main && git pull origin main
```

セッション管理:
```
/fork
```

**→ セッション終了。新しいセッションでPhase 3を開始する**

---

---

# Phase 3: notify設定 + カスタムコマンド自作

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- `config.toml` の `notify` 設定（Claude CodeのPostToolUse Hook相当）
- カスタムコマンドの自作
- `.codexignore` の整備

**セッション開始:** 新しいセッション。

**フェーズ開始時にブランチを作成（手動）:**
```bash
git checkout -b feature/phase3-notify-commands
```

---

## タスク P3-1: notify設定（タスク完了通知）

> **notifyとは:**
> Codex CLIで長時間のタスクが完了したときに通知を送る設定。
> Claude CodeのPostToolUse Hookに相当する自動アクション機能。
> `~/.codex/config.toml` の `[notify]` セクションで設定する。

```
~/.codex/config.toml に notify 設定を追加してください。

設定内容:
[notify]
on_task_complete = ["bash", "-lc", "afplay /System/Library/Sounds/Blow.aiff"]

設定後、長めのタスク（pytest の実行など）を依頼して、
完了時に通知音が鳴ることを確認してください。
notify設定の仕組みと使いどころも説明してください。
```

## タスク P3-2: config.toml の拡張設定

```
~/.codex/config.toml を以下のように拡張してください:

model = "o4-mini"
approval_policy = "suggest"
personality = "pragmatic"

[notify]
on_task_complete = ["bash", "-lc", "afplay /System/Library/Sounds/Blow.aiff"]

[mcp_servers.github]
command = "npx"
args = ["-y", "@modelcontextprotocol/server-github"]
env = { GITHUB_PERSONAL_ACCESS_TOKEN = "$GITHUB_TOKEN" }

各設定項目の意味を説明してから適用してください。
また、GITHUB_TOKEN 環境変数が ~/.zshrc に設定済みであることを確認してください。
```

## タスク P3-3: セキュリティ用カスタムコマンド（安全ガード）

```
危険なコマンドを実行しようとした時に警告を出すカスタムコマンドを作成してください。

ファイル: ~/.codex/safety-check.md
内容:
---
rm -rf を含むコマンドが提案された場合は必ず警告を表示し、
削除対象のパスを明示してから私の確認を取ってください。
また、sudo コマンドが必要な場合も理由を説明してから実行してください。
---

作成後、/safety-check を呼び出して動作を確認してください。
```

## タスク P3-4: .codexignore の整備

```
.codexignore を作成してください（作業ディレクトリ直下に配置）。
Codex CLIがコンテキストに含めるべきでないファイルを除外します。

除外すべきパス:
- .venv/
- __pycache__/
- .pytest_cache/
- *.pyc
- .DS_Store
- *.egg-info/

各除外項目の理由も説明してください。
```

## タスク P3-5: taskr-status カスタムコマンドの自作【カスタムコマンド必須】

```
カスタムコマンドを作成してください。

ファイル: ~/.codex/taskr-status.md
内容:
---
taskrプロジェクトの現在の実装状況とテストカバレッジを確認して報告する。

以下を確認してレポートせよ:
1. uv run pytest --tb=short の結果
2. uv run ruff check . の結果
3. 実装済みのCLIコマンド一覧
4. AGENTS.mdの「現在のフェーズ」を確認して次にやるべきことを提示
---

カスタムコマンド作成後、/taskr-status で呼び出して動作確認してください。
```

## タスク P3-6: フェーズ終了の半自動化【/end-phase カスタムコマンド】

毎フェーズ末にセッション管理（`/fork`）の実行タイミングを忘れがちです。
カスタムコマンドを作って半自動化しましょう。

```
以下のカスタムコマンドを作成してください。

ファイル: ~/.codex/end-phase.md
内容:
---
フェーズ完了時のセッション管理を補助する。現在の作業内容から次のアクションを整理し、
AGENTS.mdの現在フェーズを更新する。

以下のステップで処理せよ:

1. AGENTS.md を読んで「現在のフェーズ」と直近の作業内容を確認する
2. このフェーズで完了したタスクのサマリーを箇条書きで表示する
3. AGENTS.mdの「現在のフェーズ」を次のフェーズに更新してよいか確認する
   OKなら自動で更新する
4. 以下の形式でコピペしやすく出力する:

   フェーズ終了コマンド（コピペして実行してください）:
   /fork
   codex resume --last   # 次回セッション再開時に使用
---

カスタムコマンド作成後、/end-phase を実行して動作確認してください。
```

> **このカスタムコマンドの効果:**
> - AGENTS.mdの「現在のフェーズ」更新も忘れなくなる
> - フェーズ完了サマリーが自動で出力される
> - **Phase 3以降はすべてのフェーズ末に `/end-phase` を使う**

---

## ✅ Phase 3 完了チェック

- [ ] タスク完了時に通知音が鳴る（notify設定）
- [ ] config.tomlにMCP設定が追加された
- [ ] `/taskr-status` コマンドが動作する
- [ ] `/end-phase` コマンドがフェーズサマリーを出力してくれる

## Phase 3 コミット & プッシュ【/commit + 手動push/PR（定着）】

```bash
git add taskr/.codexignore AGENTS.md
# ~/.codex/ の個人設定はリポジトリに含めない
```

```
/commit
```

```bash
git push -u origin feature/phase3-notify-commands
gh pr create \
  --title "feat(phase3): add notify config and taskr-status custom command" \
  --body "config.tomlのnotify設定を追加。カスタムコマンド(/taskr-status)を自作。" \
  --base main
gh pr merge --merge
git checkout main && git pull origin main
```

セッション管理（`/end-phase` カスタムコマンドを使う）:
```
/end-phase
```
→ Codexが作業内容からフェーズサマリーを出力してくれる。提案された `/fork` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 4を開始する**

---

---

# Phase 4: MCP統合 + Subagent 初体験

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- GitHub MCP によるIssue/PR管理
- Subagent機能の基本操作（max_depthパラメータ）
- `/mention` によるコンテキスト管理

**セッション開始:** 新しいセッション。

> **Subagentとは（Codex CLI版）:**
> Codex CLIのsubagent機能は、タスクを階層的に分解して独立したエージェントに委譲する機能。
> `max_depth` パラメータで階層の深さを制御する。
> メインセッションと切り離された独自コンテキストを持つ。

**フェーズ開始時にブランチを作成（手動）:**
```bash
git checkout -b feature/phase4-mcp-subagent
```

---

## タスク P4-1: GitHub MCP のセットアップ

```
GitHub MCPをセットアップしてください。

~/.codex/config.toml の [mcp_servers.github] 設定を確認し、
接続できることを確かめてから以下を確認:
1. GitHub MCPでlearn_claudeリポジトリのIssue一覧を取得
2. Phase 3で作成したIssueをMCP経由でクローズ
```

## タスク P4-2: /mention でのコンテキスト管理

```
/mention taskr/taskr/models.py
/mention taskr/taskr/storage.py
```

→ 特定のファイルをコンテキストに追加して、より正確な回答を引き出す体験をする。

```
今追加した2つのファイルの関係性を説明してください。
storage.pyがmodels.pyのTaskクラスをどのように利用しているか分析してください。
```

## タスク P4-3: Subagent 初体験【Subagent必須】

```
subagent機能を使って調査タスクを委託してください。

依頼内容:
「clickライブラリを使ったCLIのテスト方法（pytestとの組み合わせ）を
 codebaseを調査した上でベストプラクティスを報告してください」

subagentを使い、結果をメインセッションに返してもらってください。
返ってきた結果をもとに test_cli.py を改善してください。
```

## タスク P4-4: タグフィルタリング機能の実装

```
subagentの調査結果を活用して、
taskr list --tag <タグ名> でタグ絞り込みができる機能を実装してください。

実装後:
- テストを追加
- uv run pytest で確認
```

## タスク P4-5: /commit-push-pr カスタムコマンドの作成と初体験

```
~/.codex/commit-push-pr.md を作成してください:

ファイル内容:
---
以下のステップを順番に実行してください:
1. git add -u でステージング
2. Conventional Commits形式のコミットメッセージを生成してコミット
3. 現在のブランチにpush
4. gh pr create でPullRequestを作成（タイトルとdescriptionはコミット内容から自動生成）
5. 完了したらPRのURLを表示する
---

作成後 /commit-push-pr を実行してください。
```

→ `add + commit + push + PR作成` がワンコマンドで完了する体験をする。
→ Phase 2〜3の手動操作と比べて何ステップ省略できたか確認する。

---

## ✅ Phase 4 完了チェック

- [ ] GitHub MCPでIssueを操作できた
- [ ] subagentに調査を委託して結果を受け取れた
- [ ] `taskr list --tag work` が動作する
- [ ] `/commit-push-pr` カスタムコマンドでPRを作成できた

## Phase 4 コミット & プッシュ【/commit-push-pr カスタムコマンド初体験】

ブランチ作成は手動だが、それ以降はカスタムコマンドに委譲:

```
/commit-push-pr
```

PR確認・マージ:
```bash
gh pr merge --merge
git checkout main && git pull origin main
```

セッション管理（`/end-phase` カスタムコマンドを使う）:
```
/end-phase
```
→ Codexが作業内容からフェーズサマリーを出力してくれる。提案された `/fork` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 5を開始する**

---

---

# Phase 5: 承認モード設定【3段階を体験する】

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- Suggest / Auto-Edit / Full-Auto の3段階承認モードの違い
- 用途に応じた承認モードの切り替え
- config.toml によるデフォルトモードの設定

**セッション開始:** 新しいセッション。

> **Codex CLIの承認モード（3段階）:**
> ```
> Suggest   ← 提案のみ。人間が全操作を承認してから実行（最も安全）
>              設計フェーズ・新規プロジェクト・危険な操作に適する
> Auto-Edit ← ファイル変更は自動、コマンド実行は承認が必要（バランス型）
>              開発フェーズの標準設定として推奨
> Full-Auto ← すべて自動実行（CI/CD・スクリプト実行向け）
>              信頼できる環境のみで使用すること
> ```

**フェーズ開始時にブランチを作成（手動）:**
```bash
git checkout -b feature/phase5-approval-modes
```

---

## タスク P5-1: 3段階モードの体験（Suggest）

```
/permissions
```

「Suggest」を選択してから:

```
README.mdに「インストール方法」セクションを追加してください。
uv を使った仮想環境のセットアップ手順を記載してください。
```

→ Codexがファイル変更を**提案**するが実行はしない。
→ 変更内容を確認してから承認（Approve）または拒否（Deny）する。
→ **すべての操作に承認が必要**なことを体感する。

## タスク P5-2: 3段階モードの体験（Auto-Edit）

```
/permissions
```

「Auto-Edit」を選択してから:

```
tests/test_cli.py を作成して、add と list コマンドのテストを書いてください。
click の CliRunner を使ってテストしてください。
```

→ Codexがファイル変更は**自動実行**するが、Bashコマンド実行は承認を求める。
→ ファイル作成・編集は即座に行われるが、`uv run pytest` 実行時には承認ダイアログが出ることを確認する。

## タスク P5-3: 3段階モードの体験（Full-Auto）

> ⚠️ Full-Autoは強力なモードです。信頼できるタスクにのみ使用してください。

```
/permissions
```

「Full-Auto」を選択してから:

```
以下の簡単なタスクを自動実行してください:
1. uv run ruff check . で問題を確認
2. 問題があれば uv run ruff format . で自動修正
3. uv run pytest でテストが通ることを確認
4. 結果をサマリーとして報告してください
```

→ Codexがすべての操作を**確認なしに自動実行**する。
→ 完了後に結果のサマリーが表示される。

## タスク P5-4: config.toml でデフォルトモードを設定

```
~/.codex/config.toml で approval_policy を設定してください。

開発フェーズでの推奨設定を説明した上で、
今後の標準設定を決定してください。

推奨:
- 通常開発: auto-edit（ファイル変更は自動、コマンドは確認）
- 設計検討: suggest（全操作を確認）
- CI/CD実行: full-auto（完全自動）

設定後、/status で現在のモードが反映されていることを確認してください。
```

## タスク P5-5: モード切り替えのユースケースを記録

```
AGENTS.mdに以下を追記してください。
「承認モードガイドライン」セクション:
- 各モードの使いどころ
- このプロジェクトでのデフォルト設定とその理由
- 用途別の切り替え例
```

---

## ✅ Phase 5 完了チェック

- [ ] Suggestモードで設計・提案を確認できた
- [ ] Auto-Editモードでファイル変更の自動化を体験した
- [ ] Full-Autoモードで完全自動実行を体験した
- [ ] config.tomlでデフォルトモードを設定した
- [ ] AGENTS.mdに承認モードガイドラインが記録された

## Phase 5 コミット & プッシュ【/commit-push-pr（定着）】

```
/commit-push-pr
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

セッション管理（`/end-phase` カスタムコマンドを使う）:
```
/end-phase
```
→ Codexが作業内容からフェーズサマリーを出力してくれる。提案された `/fork` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 6を開始する**

---

---

# Phase 6: Suggest Mode本格活用 + Subagent応用

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- Suggest Modeで大機能を設計してから実装するフロー
- subagentを並列で動かして設計を補強する
- 設計判断の文書化
- `/model` でのモデル切り替え（推論タスクへの対応）

**セッション開始:** 新しいセッション。

---

## タスク P6-1: Suggest Modeで大機能を設計

Suggest Modeに切り替え:
```
/permissions
```
「Suggest」を選択してから以下を投げる:

```
taskrに以下の機能を追加する計画を立ててください（コードは書かない）:
- 優先度システム (high / medium / low)
- 期限設定 (due date、YYYY-MM-DD形式)

設計書に含めること:
- データモデルの変更（Taskクラスへの追加フィールド）
- 既存データとの後方互換性戦略（既存のtasks.jsonが壊れない方法）
- CLI引数の変更（add --priority high --due 2026-03-01 など）
- テストの追加計画
- 実装の順序
```

```
/status
```
→ 設計提案の内容とトークン使用量を確認する。

## タスク P6-2: subagentによる並列調査【Subagent応用】

```
Suggest Modeの設計判断を補強するため、subagentを2つ並列で動かしてください。

subagent 1:
「CLIツールにおける優先度実装のベストプラクティスを調査してください」

subagent 2:
「Pythonのdatetime/date型の扱い方とJSONシリアライズ方法を調査してください」

2つのsubagentの調査結果をまとめて、設計書を最終化してください。
```

## タスク P6-3: /model で推論モデルに切り替えて設計レビュー

```
/model
```

→ o4-mini から o3 に切り替える（より深い推論が必要な設計検討に対応）

```
先ほどの設計書について、以下の観点でレビューしてください:
1. 後方互換性の実装方法に問題がないか
2. テスト追加計画に抜け漏れがないか
3. 実装順序の適切さ

問題があれば設計書を修正してください。
```

レビュー完了後:
```
/model
```
→ o3 から o4-mini に戻す（コスト最適化）

## タスク P6-4: Auto-Editモードに切り替えて実装

承認モードをAuto-Editに変更:
```
/permissions
```
「Auto-Edit」を選択してから:

```
設計書に従って優先度と期限機能を実装してください。
実装順序: models.py → storage.py → cli.py → テスト追加
各ステップで uv run pytest を実行して既存テストが壊れていないか確認してください。
```

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
- [ ] subagentを2つ並列で動かせた
- [ ] `/model` でo3に切り替えて設計レビューできた
- [ ] `taskr add "タスク" --priority high --due 2026-03-15` が動作する
- [ ] AGENTS.mdに設計判断が記録された

## Phase 6 コミット & プッシュ【プロンプト一発でブランチ〜PR（フル自動化）】

ブランチ作成もCodex CLIに指示する:

```
feature/phase6-priority-duedate ブランチを作成して、
今フェーズの実装内容をコミット・プッシュして、PRを作成してください。
PRのタイトルとdescriptionは変更内容から自動生成してください。
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

セッション管理（`/end-phase` カスタムコマンドを使う）:
```
/end-phase
```
→ Codexが作業内容からフェーズサマリーを出力してくれる。提案された `/fork` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 7を開始する**

---

---

# Phase 7: Subagent Teams 基礎

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- max_depthパラメータを使った階層的なsubagent実行
- 2つのsubagentを並列で動かす体験
- subagent間の成果物の統合
- バックグラウンドタスク実行

**セッション開始:** 新しいセッション。

> **Subagent Teams（Codex CLI版）とは:**
> Codex CLIのsubagent機能は `max_depth` パラメータで階層的なタスク分解が可能。
> メインエージェントが複数のsubagentをオーケストレートし、
> 各subagentが独立したコンテキストで並列作業を行う。
>
> **Claude CodeのAgent Teamsとの対応:**
> - Claude Code: tmuxベースの複数Claude Codeプロセス
> - Codex CLI: max_depthパラメータとcodex execの並列実行で同等の効果を実現

---

## タスク P7-1: subagent機能の仕組みを理解

```
Codex CLIのsubagent機能について説明してください:
1. どのような仕組みで動くか（max_depthパラメータの役割）
2. 並列subagentの使いどころ
3. subagent間でコンテキストを共有する方法
4. このtaskrプロジェクトでsubagentを使うのに適したシナリオを3つ提案してください
```

## タスク P7-2: 2subagent並列実行体験【Subagent必須】

```
subagent機能を使って以下を並列実行してください:

subagent 1（テスト担当）:
- test_models.py の作成（Taskクラスの全メソッドをテスト）
- test_storage.py の更新（priority, due_dateのテストを追加）

subagent 2（ドキュメント担当）:
- README.md の更新（全コマンドの使い方を追記）
- CHANGELOG.md の作成（バージョン履歴の開始）

2つのsubagentが完了したら結果を統合して、テストが通ることを確認してください。
```

## タスク P7-3: max_depthを使った階層的タスク分解

```
max_depth=2 を使って、以下の階層的なタスクを実行してください:

レベル1（メインタスク）: taskrのコード品質を改善する
  レベル2-A（subagent）: ruff でリントエラーを全修正する
  レベル2-B（subagent）: pytest カバレッジを60%以上に引き上げる

各subagentが完了したらメインタスクとして結果をまとめて報告してください。
```

## タスク P7-4: codex exec でバックグラウンドタスク実行

> **codex exec とは:**
> Codex CLIの非インタラクティブモードでのコマンド実行機能。
> バックグラウンドで長時間タスクを実行する際に使う。
> Claude CodeにはないCodex CLI固有の機能。

別ターミナルで以下を実行:
```bash
# バックグラウンドでカバレッジ計測を実行
codex exec "uv run pytest --cov=taskr --cov-report=term-missing でカバレッジを計測して結果をレポートしてください"

# 別のターミナルでリント全体チェックを並列実行
codex exec "uv run ruff check . の結果を分析して、修正が必要な問題点をリストアップしてください"
```

メインセッションでは:
```
バックグラウンドでcodex execを実行しています。
その間に AGENTS.md に Phase 7 の学習記録を追記してください。
subagent機能で学んだことと、codex execとの使い分けをまとめてください。
```

---

## ✅ Phase 7 完了チェック

- [ ] 2つのsubagentが並列で異なるファイルを作業した
- [ ] max_depthを使って階層的なタスク分解を体験した
- [ ] codex exec でバックグラウンド実行を体験した
- [ ] AGENTS.mdにPhase 7の学習記録が追記された

## Phase 7 コミット & プッシュ【Subagent + フル自動GitHub Flow】

```
subagentでの作業が完了しました。
feature/phase7-subagent-teams ブランチを作成して、
テストとドキュメントの変更をまとめてコミット・プッシュ・PR作成してください。
PR本文には「subagentで並列実装した内容の概要」を自動で記述してください。
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

セッション管理（`/end-phase` カスタムコマンドを使う）:
```
/end-phase
```
→ Codexが作業内容からフェーズサマリーを出力してくれる。提案された `/fork` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 8を開始する**

---

---

# Phase 8: Subagent Teams応用 + codex exec CI/CD化

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- `/commit-push-pr` カスタムコマンドによるワークフロー確立
- 複数subagentの協調とコンテキスト統合
- GitHub Actions CI/CD の設定
- `codex exec` を使ったCI/CDスクリプト化

**セッション開始:** 新しいセッション。

---

## タスク P8-1: subagentでレポート機能実装【Subagent本格活用】

```
subagent機能を使って「taskr report」コマンドを実装してください。

要件:
- taskr report で統計情報を表示
- 完了/未完了タスク数、優先度別集計、タグ別集計

subagent分担:
- subagent A: storage.pyに集計ロジックを追加
- subagent B: cli.pyにreportコマンドを追加
- subagent C: テストとREADME更新

各subagentの作業完了後に統合してください。
```

## タスク P8-2: コンテキスト統合の練習

```
subagentを使って、以下を並列実装してください:

subagent 1: cli.py に taskr export コマンドを追加（JSON/CSV出力）
subagent 2: cli.py に taskr import コマンドを追加（JSONインポート）

両subagentが同じファイルを変更するため、
統合時にコンテキストのマージが必要になります。
マージ後に uv run pytest でテストが通ることを確認してください。
```

## タスク P8-3: GitHub Actions CI設定

```
.github/workflows/ci.yml を作成してください:

トリガー: push, pull_request（mainブランチ）
jobs:
- test: Python 3.11, 3.12 のマトリックスでpytest実行
- lint: uv run ruff check
- coverage: カバレッジ80%以上を要件に

ワークフローファイル作成後にプッシュして、
GitHub MCPでActions実行状況を確認してください。
```

## タスク P8-4: codex exec でCI/CDスクリプト化【codex exec必須】

> **このタスクの目標:**
> `codex exec` コマンドを使ってCodex CLIをCI/CDパイプラインの一部として活用する。
> これはCodex CLI固有の機能で、Claude Codeにはない強力な自動化手段。

```bash
# コードレビュースクリプトとして活用
codex exec "以下を実行してください:
1. git diff main...HEAD でこのブランチの変更をすべて確認する
2. 変更内容のコードレビューを実施（品質・セキュリティ・パフォーマンスの観点）
3. 問題点があれば重要度（高/中/低）付きでリストアップする
4. 問題がなければ 'LGTM' と出力する"
```

```bash
# Full-Autoモードでのリリースチェック
codex exec --full-auto "以下のリリース前チェックを自動実行してください:
1. uv run pytest --tb=short（全テスト実行）
2. uv run ruff check .（リントチェック）
3. uv run python -m taskr --help（ヘルプ表示確認）
4. 問題がなければ '✅ リリース準備完了' と出力する
問題があれば '❌ [問題内容]' と出力する"
```

## タスク P8-5: シェルスクリプトにcodex execを組み込む

```bash
# リリース前自動チェックスクリプトを作成してください
mkdir -p scripts
cat > scripts/pre-release-check.sh << 'EOF'
#!/bin/bash
set -e

echo "=== taskr リリース前チェック ==="

# codex exec でAI支援コードレビュー
codex exec --full-auto "
git diffでmainからの変更を分析し、リリースに問題がないか確認してください。
問題があれば具体的な箇所と修正方法を出力してください。
"

echo "=== チェック完了 ==="
EOF
chmod +x scripts/pre-release-check.sh
```

```
scripts/pre-release-check.sh を作成して、
codex exec を組み込んだリリース前自動チェックを実装してください。
スクリプトを実行して動作確認してください。
```

---

## ✅ Phase 8 完了チェック

- [ ] subagentを使って3つの並列タスクを実行した
- [ ] GitHub ActionsのCIが動作している
- [ ] `codex exec` でCI/CD的なタスクを自動実行できた
- [ ] codex execをシェルスクリプトに組み込めた

## Phase 8 コミット & プッシュ【GitHub MCP + Subagent完全統合】

```
subagentで実装したすべての変更を
feature/phase8-advanced ブランチにまとめてください。
その後:
1. /commit-push-pr でコミット・プッシュ・PR作成
2. GitHub MCPを使ってPRに "enhancement" ラベルを付与
3. PR descriptionにsubagentの作業分担を自動で記録

これをすべてCodex CLIにやってもらう（人間は最終確認のみ）。
```

```bash
gh pr merge --merge
git checkout main && git pull origin main
```

セッション管理（`/end-phase` カスタムコマンドを使う）:
```
/end-phase
```
→ Codexが作業内容からフェーズサマリーを出力してくれる。提案された `/fork` コマンドをコピペして実行する。

**→ セッション終了。新しいセッションでPhase 9を開始する**

---

---

# Phase 9: 総仕上げ【全機能統合 + v1.0.0リリース】

> ⚠️ このフェーズは新しいCodexセッションで開始すること

**このフェーズで習得すること:**
- `/revise-agents-md` カスタムコマンドによる最終整備
- config.toml の最終カスタマイズ
- GitHub Releasesへの公式リリース
- 学習の振り返り

**セッション開始:** 新しいセッション。

---

## タスク P9-1: AGENTS.md の最終整備

まずカスタムコマンドを作成:

```bash
cat > ~/.codex/revise-agents-md.md << 'EOF'
AGENTS.mdを現在のプロジェクト状態に合わせて更新してください。

以下の構成で整備すること:
1. プロジェクト概要と目的
2. セットアップ手順（環境構築の要約）
3. テストコマンドの一覧
4. 全コマンド使い方ガイド
5. アーキテクチャ説明（ファイル構成と役割）
6. 設計決定ログ（全フェーズ分）
7. Codex CLIを使った開発のコツ（このプロジェクトで学んだこと）

次のCodexセッションで即座に開発を再開できるレベルに整えること。
EOF
```

```
/revise-agents-md
```

以下の構成でAGENTS.mdを完全版に整備してください:
```
1. プロジェクト概要と目的
2. セットアップ手順（環境構築の要約）
3. 全コマンド使い方ガイド
4. アーキテクチャ説明（ファイル構成と役割）
5. 設計決定ログ（全フェーズ分）
6. Codex CLIを使った開発のコツ（このプロジェクトで学んだこと）

次のCodexセッションで即座に開発を再開できるレベルに整えること。
```

## タスク P9-2: config.toml の最終カスタマイズ

```
~/.codex/config.toml を最終版に整えてください。
これまでのフェーズで学んだ設定を統合して、
最適な開発環境設定を作成してください。

含めるべき設定:
- model（デフォルトモデル）
- approval_policy（デフォルト承認モード）
- personality
- notify（タスク完了通知）
- MCP設定（GitHub MCP）

各設定の選択理由もコメントとして記載してください。
```

```
/status
```
→ 最終的な設定が正しく反映されていることを確認する。

## タスク P9-3: インタラクティブモードの実装（Subagent総動員）

```
subagentを使って taskr -i（インタラクティブモード）を実装してください。

バックグラウンドsubagent:
TUIライブラリの選定調査（questionary, prompt_toolkit, rich を比較して推奨を報告）

並列subagent:
- subagent 1: TUIライブラリ導入と基本UI実装
- subagent 2: キーバインドとナビゲーション
- subagent 3: 既存CLIとの統合とテスト

uv add でライブラリを追加してから実装してください。
```

## タスク P9-4: codex exec でリリース前最終チェック

```bash
# Phase 8で作成したスクリプトを実行
bash scripts/pre-release-check.sh
```

```bash
# Full-Autoモードで最終チェック
codex exec --full-auto "
taskr v1.0.0 のリリース前最終チェックを実施してください:
1. uv run pytest --tb=short でテスト全通過を確認
2. uv run ruff check . でリントエラーなしを確認
3. uv run python -m taskr --version でバージョン表示を確認
4. CHANGELOG.md が適切に更新されているか確認
すべて問題なければ '✅ v1.0.0 リリース準備完了' と出力してください。
"
```

## タスク P9-5: v1.0.0 リリース

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

## タスク P9-6: 学習振り返り

```
このカリキュラムを通じた学びをAGENTS.mdに追記してください。

「学習まとめ」セクション:
1. 各フェーズで習得したCodex CLI機能
2. カスタムコマンド / Subagent / codex exec それぞれの使いどころと実感
3. バイブコーディングで効果的だったプロンプトパターン
4. 仕事で活用できるフロー（Suggest Mode・コミュニケーション保存・codex exec CI/CD化）
5. GitHub Flow自動化の段階的変化（手動→カスタムコマンド→完全自動）の振り返り
6. Claude CodeとCodex CLIの使い分けについて感じたこと

GitHub Issueで今後のロードマップを5件作成してください。
```

---

## ✅ Phase 9 完了チェック

- [ ] `uv run python -m taskr --version` が `1.0.0` を返す
- [ ] GitHub Releasesに v1.0.0 が公開されている
- [ ] AGENTS.mdの学習まとめが完成している
- [ ] config.tomlが最終版に整備されている
- [ ] codex exec によるリリース前チェックが自動化されている

## Phase 9 最終コミット & プッシュ【完全自動化の総仕上げ】

GitHub Flowの全工程をプロンプト一発で完結させる:

```
以下をすべて自動で実施してください:
1. feature/phase9-final-release ブランチを作成
2. インタラクティブモードの変更をコミット
3. /commit-push-pr でPR作成
4. GitHub MCPでPRを確認・マージ
5. mainにマージ後、git tag v1.0.0 を付与
6. git push origin main --tags
7. GitHub MCPでReleasesを作成（CHANGELOGから自動生成）

人間が行うのは最終確認のみ。
```

セッション管理（`/end-phase` カスタムコマンドを使う）:
```
/end-phase
```
→ Codexが作業内容からフェーズサマリーを出力してくれる。提案された `/fork` コマンドをコピペして実行する。

---

---

# カリキュラム完了

おめでとうございます。以下の機能を習得しました:

| 習得項目 | 確認方法 |
|---------|---------|
| **カスタムコマンド** | `/commit`, `/commit-push-pr`, `/revise-agents-md`, カスタム `/taskr-status` |
| **Subagent** | Phase 4, 6, 7, 9 で並列調査・実装を委託した |
| **codex exec** | Phase 8〜9 でCI/CDスクリプトとして活用した |
| **Suggest Mode** | Phase 1, 6 で設計書を作成してから実装した |
| **コミュニケーション保存** | AGENTS.md + /fork + /resume で全フェーズを引き継いだ |
| **GitHub Flow** | 手動操作 → カスタムコマンド自動化 → 完全自動化の変遷を体験した |

**taskr v1.0.0 のリポジトリを公開してカリキュラム完了！**

---

## Claude Code版との主な差異

| 機能・概念 | Claude Code版 | Codex CLI版 |
|-----------|-------------|------------|
| **プロジェクト設定ファイル** | `CLAUDE.md` | `AGENTS.md` |
| **グローバル設定** | `~/.claude/settings.json` | `~/.codex/config.toml` |
| **Plan Mode** | `claude --plan` または Shift+Tab×2 | Suggest Mode（`/permissions` → Suggest） |
| **設計状態の確認** | Plan Modeの画面内 | `/status` コマンド |
| **承認モード** | スマートサンドボックス（暗黙的） | Suggest / Auto-Edit / Full-Auto の明示的3段階 |
| **カスタムコマンド** | `~/.claude/skills/<name>/SKILL.md` | `~/.codex/<name>.md`（ファイル名=コマンド名） |
| **Hooks（自動アクション）** | PostToolUse / PreToolUse Hook | `config.toml` の `[notify]` セクション |
| **セッション名付け** | `/rename <name>` | セッション名管理なし（`/fork` で状態保存） |
| **セッション再開** | `/resume` | `/resume` または `codex resume --last` |
| **セッション状態保存** | `/rename` によるブックマーク | `/fork` による明示的な分岐・保存 |
| **MCP設定場所** | `.mcp.json` | `config.toml` の `[mcp_servers.<name>]` |
| **CI/CD実行** | 非対応（スクリプト内でclaudeを呼ぶ形） | `codex exec` コマンド（--full-autoオプション付き） |
| **コンテキスト追加** | ファイルをドラッグ or コマンドライン引数 | `/mention [path]` コマンド |
| **並列エージェント** | Agent Teams（tmuxベース） | subagentのmax_depth + codex execの並列実行 |
| **差分確認** | 都度確認 | `/diff` コマンドでGit差分を明示的に確認 |
| **インストール方法** | `npm i -g @anthropic-ai/claude-code` | `npm i -g @openai/codex` または `brew install --cask codex` |
| **ログイン方法** | `claude login`（ブラウザ認証） | `OPENAI_API_KEY` 環境変数 |
| **Windows対応** | ネイティブ対応 | WSL2推奨（実験的） |
