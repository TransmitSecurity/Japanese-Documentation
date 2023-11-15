# Transmit Security Platform サンプルアプリケーション利用ガイド

## はじめに
- 本ドキュメントは[Transmit Security Platform サンプルアプリケーション](https://github.com/TransmitSecurity/ciam-expressjs-vanilla-samples)の利用手順をまとめます。最新の情報はGitHubの内容を確認してください

> 本ドキュメントはメーカーオフィシャルドキュメントではありません
> 最新の情報、サポート情報はメーカードキュメントを参照してください

### 事前準備・前提

- サンプルアプリケーションの実行には、Transmit Security PlatformのPortal画面でアプリケーションの登録が必要です
- サンプルアプリケーションはNode.jsで実装されています。各種アプリケーションの動作確認でJavascriptに関する知識が必要となる場合があります
- サンプルアプリケーションは主に以下のいずれかで動作を確認いただけます。詳細はセットアップ手順を参照してください
  - [GitHubのCodeSpaceで実行](./docs/setup.md#githubのcodespaceで実行)
  - [RepositoryをCloneし、ローカルで実行](./docs/setup.md#ローカル環境で実行)
  - [Docker Container を利用し、ローカルで実行](./docs/setup.md#docker-containerで実行)
- その他、本ドキュメントでは以下が必要となります
  - インターネットに接続可能な端末
  - ブラウザ
  - 送受信可能なメールアドレス
  - 通話可能な電話番号
  - 手順に応じた簡易なCLI操作・ファイル編集

### 構成

## 手順

#### 1. [サンプルアプリケーション実行環境のセットアップ](./docs/setup.md)
#### 2. 各種サンプルアプリケーション
動作確認をしたいアプリケーション・機能の手順に従って操作してください

- Detection and Response Service
  - [Detection and Response Service](./docs/password-authentication-drs.md)

- Authentication
  - [Password Authentication](./docs/password-authentication.md)
  - [Email OTP Authentication](./docs/login-with-email.md)
  - [Email Magiclink Authentication](./docs/login-with-magiclink.md)
  - [SMS OTP Authentication](./docs/login-with-sms.md)

- Backend Authentication
  - [Backend Email OTP Authentication](./docs/login-with-be-email.md)
  - [Backend Email Magiclink Authentication](./docs/login-with-be-magiclink.md)
  - [Backend SMS OTP Authentication](./docs/login-with-be-sms.md)

- IDV
  - [Hosted IDV(ID Verification)](./docs/hosted-idv.md)

## メーカードキュメント
- [Developer TransmitSecurity](https://developer.transmitsecurity.com/)
  - ``Guides`` : 機能紹介
  - ``APIs`` : API定義、サンプルコード
  - ``SDKs`` : SDK詳細

