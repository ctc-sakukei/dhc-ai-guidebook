# Claude Code を Amazon Bedrock 経由で利用する手順（WSL / Linux）

## 前提

- 中国の携帯電話番号では **Anthropic アカウントの直接作成ができない ** (SMS認証用番号として登録不可)
- そのため、今回のBootcampでは **Amazon Bedrock 経由で Claude Code を利用**する
- 別途案内している手順に従い、WSLへのClaude Code インストールまで **事前に完了済み**であること

---

## 全体の流れ

1. AWS CLI のインストール
2. AWS 認証情報（Access Key）の設定
3. AssumeRole プロファイル設定
4. Bedrock 利用向け環境変数設定（一時）
5. Claude Code 起動・設定確認
6. チャット実行テスト
7. 環境変数の恒久化（.bashrc）

---

## 1. AWS CLI のインストール（WSL）

### root ユーザで実行

```bash
wsl -u root

sudo apt update
sudo apt install -y curl unzip

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

### root ユーザからログアウト

```bash
exit
```

---

## 2. AWS 認証情報の設定（一般ユーザ）

```bash
wsl
aws configure
```

入力内容：

```text
AWS Access Key ID:     <アクセスキー>
AWS Secret Access Key: <シークレットアクセスキー>
Default region name:   ap-northeast-1
Default output format: json
```

### 設定確認

```bash
aws sts get-caller-identity
```

IAM ユーザ情報が表示されれば OK：

```json
{
  "UserId": "AIDA*****************",
  "Account": "870057116847",
  "Arn": "arn:aws:iam::870057116847:user/your-user"
}
```

---

## 3. AssumeRole プロファイル設定

### `~/.aws/config` を編集

```bash
vi ~/.aws/config
```

以下を追記：

```ini
[profile bedrock-dev]
role_arn = arn:aws:iam::870057116847:role/XinhuaxinBedrockInvoke
source_profile = default
region = ap-northeast-1
output = json
```

### AssumeRole 動作確認

```bash
AWS_PROFILE=bedrock-dev aws sts get-caller-identity
```

- `Arn` に **assumed-role** が表示されていれば OK

---

## 4. Bedrock 利用向け環境変数設定（一時）

```bash
# Bedrock 経由で Claude Code を利用
export CLAUDE_CODE_USE_BEDROCK=1

# AWS プロファイル指定
export AWS_PROFILE=bedrock-dev

# 利用リージョン・モデル指定
export AWS_REGION=ap-northeast-1
export ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION=ap-northeast-1
export ANTHROPIC_MODEL='jp.anthropic.claude-sonnet-4-5-20250929-v1:0'
export ANTHROPIC_DEFAULT_OPUS_MODEL='global.anthropic.claude-opus-4-5-20251101-v1:0'
export ANTHROPIC_DEFAULT_SONNET_MODEL='jp.anthropic.claude-sonnet-4-5-20250929-v1:0'
export ANTHROPIC_DEFAULT_HAIKU_MODEL='jp.anthropic.claude-haiku-4-5-20251001-v1:0'

# 出力トークン設定（Bedrock 推奨）
export CLAUDE_CODE_MAX_OUTPUT_TOKENS=4096
export MAX_THINKING_TOKENS=1024
```

---

## 5. Claude Code 起動・設定確認

### 起動

```bash
claude
```

- ログイン要求が表示されず、そのままプロンプト画面に遷移すれば OK

### プロバイダ確認

Claude Code 上で以下を実行：

```text
/status
```

- `API provider: AWS Bedrock` になっていることを確認

---

## 6. チャット実行テスト

以下のプロンプトを送信：

```text
Bedrock経由でのClaude Codeへのチャット送信テストです。
もしこのチャットがBedrock経由であれば、実行時のモデル情報・呼出元AWSリージョンと
「Bedrock経由実行 OK」の文字列を返してください。

Claude Code直接実行であれば「Bedrock経由実行 NG」を返してください。
```

期待される出力：

```text
- Bedrock経由実行 OK
- モデル: Claude Sonnet 4.5
- リージョン: ap-northeast-1
```

- 上記以外の文字列も追加で出力されることがあるが、上記内容が含まれれば **実行確認 OK** と判断できる

---

## 7. 環境変数の恒久化（.bashrc）

Claude Code を一度終了後、以下を実施。

```bash
vi ~/.bashrc
```

末尾に追記：

```bash
############################################
# Environment Variables for Claude Code (Bedrock)
############################################

export CLAUDE_CODE_USE_BEDROCK=1
export AWS_PROFILE=bedrock-dev

export AWS_REGION=ap-northeast-1
export ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION=ap-northeast-1
export ANTHROPIC_MODEL='jp.anthropic.claude-sonnet-4-5-20250929-v1:0'
export ANTHROPIC_DEFAULT_OPUS_MODEL='global.anthropic.claude-opus-4-5-20251101-v1:0'
export ANTHROPIC_DEFAULT_SONNET_MODEL='jp.anthropic.claude-sonnet-4-5-20250929-v1:0'
export ANTHROPIC_DEFAULT_HAIKU_MODEL='jp.anthropic.claude-haiku-4-5-20251001-v1:0'

export CLAUDE_CODE_MAX_OUTPUT_TOKENS=4096
export MAX_THINKING_TOKENS=1024
```

### 反映

```bash
source ~/.bashrc
```

---