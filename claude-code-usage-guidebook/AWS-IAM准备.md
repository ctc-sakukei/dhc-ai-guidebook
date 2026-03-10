# AWS 初回セットアップ手順（Bedrock / Claude Code 利用前）

本手順は、Amazon Bedrock（Claude Code）利用前に
**AWS IAMユーザの初期設定を行い、アクセスキーを発行する** ためのものである

---

## 0. 事前に配布されている情報

以下は個別に配布

- AWS コンソールログインURL
  https://aws-ita-support.signin.aws.amazon.com/console
- IAMユーザー名
- 初期パスワード（ワンタイム）

---

## 1. AWSコンソールへログイン

上記URLへアクセスし、
IAMユーザー名・初期パスワードでログイン

---

## 2. パスワード変更（必須）

初回ログイン後、パスワード変更を行うこと

- 他サービスとの使い回しは禁止
- 第三者との共有は禁止

変更後、再ログインできることを確認

---

## 3. MFA（多要素認証）の設定【必須】

コンソールから、以下の手順でMFAを有効化する

1. 右上のユーザー名 → **Security credentials**
2. **Multi-factor authentication (MFA)** → Assign MFA device
3. 認証アプリ等を利用してMFAを設定

### 注意

- **MFA設定が完了するまで、アクセスキーは作成しないこと**
- 設定後、「MFA device assigned」と表示されることを確認

---

## 4. アクセスキーの発行（MFA完了後）

MFA設定完了後、以下の手順でアクセスキーを作成

1. **Security credentials**
2. **Access keys** → Create access key
3. 用途は「CLI」を選択
4. Access Key / Secret Key を取得

### 重要

- アクセスキー・シークレットキーは再表示不可のため、CSVをその場で保存すること
- アクセスキー・シークレットキーは他者との共有やオンラインへのアップは厳禁