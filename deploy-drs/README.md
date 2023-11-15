# Transmit Security Platform DRS Deploy Guide

## はじめに
- 本ドキュメントは[Transmit Security Guides: Detection and Response](https://developer.transmitsecurity.com/guides/risk/overview/)のQuick Startsの内容に従い、利用方法につて解説します。最新の情報はメーカーページを参照してください

> 本ドキュメントはメーカーオフィシャルドキュメントではありません
> 最新の情報、サポート情報はメーカードキュメントを参照してください

### 事前準備・前提

- Transmit Security PlatformのPortal画面でアプリケーションの登録が必要です
- 各種利用方法で利用するプラットフォームにより必要となる知識が異なります。詳細については各サービスのドキュメントを参照してください
- その他、本ドキュメントでは以下が必要となります
  - インターネットに接続可能な端末
  - ブラウザ
  - 手順に応じた簡易なCLI操作・ファイル編集

### 構成

  <img src="https://developer.transmitsecurity.com/9f9efa72d192d1ea6ef27ad5e2bddf35/how_it_works.png" width="500"/>

## 手順

1. [Transmit Security Platformでアプリケーションのセットアップ](./docs/setup.md)
1. サンプルアプリケーションの実行・DRSの基本動作の確認方法
   - TransmitSecurityが提供する[サンプルアプリケーション](https://github.com/TransmitSecurity/ciam-expressjs-vanilla-samples/tree/main)を用いた、DRSの基本動作の確認手順です
   1. [サンプルアプリケーションによるDRSの動作確認](../vanilla-sample/docs/password-authentication-drs.md)

1. 各種導入・利用方法
   - Detection and Response Serviceの各種導入手順です
   1. [JavaScript SDK](./docs/drs-jssdk.md)
   1. [Google Tag manager](./docs/drs-gtm.md)
   1. [Cloudflare Workers](./docs/drs-cloudflareworker.md)
   1. [Android SDK](./docs/drs-androidsdk.md)
   1. [iOS SDK](./docs/drs-iossdk.md)

## メーカードキュメント
- [Developer TransmitSecurity](https://developer.transmitsecurity.com/)
  - ``Guides`` : 機能紹介
  - ``APIs`` : API定義、サンプルコード
  - ``SDKs`` : SDK詳細

