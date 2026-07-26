# GitHub Copilot CLI のインストールと実行方法

このドキュメントでは、スタンドアロン版 GitHub Copilot CLI を導入し、認証して使い始めるまでの手順を説明します。

## 1. 前提条件

- 有効な GitHub Copilot サブスクリプションを利用できる
- ターミナルを利用できる
- Windows の場合は PowerShell 6 以降を利用できる

組織またはエンタープライズ経由で Copilot を利用している場合、管理者が Copilot CLI を無効にしていると使用できません。

## 2. Copilot CLI をインストールする

環境に合う方法を 1 つ選びます。インストール後に使うコマンドは `copilot` です。

最初に、既存インストールの有無を確認します。

```bash
copilot version
```

バージョンが表示される場合は、すでにインストール済みです。表示されない場合は、以下のいずれかで導入してください。

### 2.1 方法の選び方

- macOS または Linux で Homebrew を使っている: Homebrew を推奨
- Windows で WinGet を使っている: WinGet を推奨
- Homebrew や WinGet が使えない: npm またはインストールスクリプトを利用

`gh copilot` は GitHub CLI 経由の実行方法であり、この手順で導入するスタンドアロン `copilot` コマンドとは別です。

### macOS または Linux（Homebrew）

```bash
brew install --cask copilot-cli
```

### Windows（WinGet）

```powershell
winget install GitHub.Copilot
```

### npm（macOS、Linux、Windows）

この方法には Node.js 22 以降が必要です。

```bash
node --version
npm --version
```

```bash
npm install -g @github/copilot
```

`~/.npmrc` で `ignore-scripts=true` を設定している場合は、次のコマンドを使います。

```bash
npm_config_ignore_scripts=false npm install -g @github/copilot
```

### macOS または Linux（インストールスクリプト）

```bash
curl -fsSL https://gh.io/copilot-install | bash
```

`curl` が使えない場合は、次のコマンドを使えます。

```bash
wget -qO- https://gh.io/copilot-install | bash
```

インストール後に確認します。

```bash
copilot version
```

## 3. 認証する

インストール後、バージョンを確認します。

```bash
copilot version
```

対話的に認証するには、次のコマンドを実行し、ブラウザで表示される案内に従います。

```bash
copilot login
```

または、`copilot` を起動してから `/login` を実行しても認証できます。

```bash
copilot
```

```text
/login
```

GitHub CLI をインストール済みで認証している場合、Copilot CLI は `gh` のトークンを認証情報のフォールバックとして使用できます。確認するには次を実行します。

```bash
gh auth status
```

認証後の確認として、`copilot` を起動して短い質問を送ると動作確認できます。

```text
このリポジトリの目的を1文で説明してください
```

## 4. 実行する

作業対象のリポジトリへ移動してから起動します。

```bash
cd path/to/repository
copilot
```

初回起動時には、現在のディレクトリを信頼するか確認されます。Copilot CLI はこのディレクトリ以下のファイルを読み取り、変更、コマンド実行することがあるため、信頼できるディレクトリでのみ続行してください。

起動後に、目的を自然言語で入力します。

```text
このリポジトリの構成を説明してください
```

Copilot CLI がファイル変更やコマンド実行を伴うツールを使う場合、実行前に許可を求めます。内容を確認して許可または拒否してください。

## 5. よく使うコマンド

```bash
copilot help
copilot version
copilot update
copilot login
```

対話セッションでは、以下のコマンドも利用できます。

```text
/login
/logout
/user
/help
```

## 6. 非対話環境で認証する

CI/CD、コンテナ、ヘッドレス環境では、`COPILOT_GITHUB_TOKEN`、`GH_TOKEN`、`GITHUB_TOKEN` の順で認証トークンを参照します。Fine-grained personal access token を使う場合は、個人アカウントで作成し、**Copilot Requests** 権限を付与します。

```bash
export COPILOT_GITHUB_TOKEN=github_pat_...
copilot
```

環境変数のトークンは、保存済みの OAuth トークンより優先されます。意図しないアカウントが使われる場合は、設定済みの `COPILOT_GITHUB_TOKEN`、`GH_TOKEN`、`GITHUB_TOKEN` を確認してください。

## 7. トラブルシュート

### `copilot: command not found`

- インストールが完了しているか確認してください。
- インストール先のディレクトリが `PATH` に含まれているか確認し、新しいターミナルを開いてから `copilot version` を再実行してください。

### `npm install -g @github/copilot` で `EACCES` が出る

グローバルインストール先がシステムディレクトリ（例: `/usr/lib/node_modules`）になっているため、一般ユーザー権限では書き込めない状態です。`sudo npm install -g ...` は避け、ユーザー領域へ npm の prefix を変更してください。

```bash
mkdir -p "$HOME/.local/npm-global"
npm config set prefix "$HOME/.local/npm-global"
echo 'export PATH="$HOME/.local/npm-global/bin:$PATH"' >> "$HOME/.bashrc"
source "$HOME/.bashrc"
npm install -g @github/copilot
copilot version
```

zsh を使っている場合は、`~/.bashrc` ではなく `~/.zshrc` に追記してください。

`~/.npmrc` に `ignore-scripts=true` を設定している場合は、次を実行してください。

```bash
npm_config_ignore_scripts=false npm install -g @github/copilot
```

### 認証に失敗する

- `copilot login` を実行し、ブラウザでの認証を完了してください。
- `gh` の認証情報を使う場合は、`gh auth status` でログイン状態を確認してください。
- 組織で SAML SSO を利用している場合は、ブラウザ上で組織へのアクセスを承認してください。

### `npm install -g @github/copilot` で Node.js バージョンエラーが出る

- `node --version` を確認し、22 未満の場合は Node.js を更新してください。
- 更新後に `npm install -g @github/copilot` を再実行してください。

### Copilot CLI を利用できない

- GitHub Copilot の利用資格があることを確認してください。
- 組織またはエンタープライズの管理者が Copilot CLI を無効にしていないか確認してください。

## 8. 公式ドキュメント

- [GitHub Copilot CLI のインストール](https://docs.github.com/ja/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli)
- [GitHub Copilot CLI の認証](https://docs.github.com/ja/copilot/how-tos/copilot-cli/set-up-copilot-cli/authenticate-copilot-cli)
- [GitHub Copilot CLI の使用](https://docs.github.com/ja/copilot/how-tos/copilot-cli/use-copilot-cli/overview)
