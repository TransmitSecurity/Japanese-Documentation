
# Passkey Authentication

## 対象サンプルアプリケーション
- [passkey-authentication](https://github.com/TransmitSecurity/ciam-expressjs-vanilla-samples/tree/main/passkey-authentication)
- このサンプルアプリケーションはユーザ名・パスワードを利用した認証を確認することができます

## はじめに
- 本ドキュメントではサンプルアプリケーションの利用に関する手順を示します
- サンプルアプリケーションを[GitHubのCodeSpaceで実行](./setup.md#githubのcodespaceで実行)した際の手順を示しています。試される環境に合わせて適宜アクセスするURLなど変更して操作ください

### 事前準備・前提
- 本ドキュメントでは以下が必要となります
  - インターネットに接続可能な端末
  - ブラウザ
  - 手順に応じた簡易なCLI操作・ファイル編集
- 動作確認端末は生体認証(TouchIDなど)を備えているもの

## サンプルアプリケーションの実行
```
SAMPLE=passkey-authentication yarn start
```

## 動作確認

### アプリケーション利用手順

#### Passkeyの登録

- ブラウザでサンプルアプリケーション`https://<codespace>-8080.app.github.dev`に接続します

- アプリケーション下部の`Sign up`をクリックしてください

  <img src="../images/ciam-vanilla-passkey-auth-01-app01.png" width="400"/>

- こちらのテストでは`username`に以下を入力し、`Register Passkey`をクリックします
  - `username` : test-user-01

  <p><img src="../images/ciam-vanilla-passkey-auth-01-app02.png" width="400"/></p>

- 対象のサイトに対しPasskeyを作成するか確認のポップアップが表示されます。`Continue`をクリックします

  <p><img src="../images/ciam-vanilla-passkey-auth-01-app03.png" width="200"/></p>

> [!NOTE]
> 本手順はGoogle Chromeで操作をしています。Google ChromeはデフォルトでGoogle ChromeのKeychainにPasskeyを登録します。ChromeではiCloud Keychainに登録することも可能です。そちらを希望する場合には、`User a different passkey`をクリックし、`iCloud Keychain`を選択してください
> 
> <p><img src="../images/ciam-vanilla-passkey-auth-01-keychain-select01.png" width="200"/></p>

- 対象のサイトに紐づくPasskeyの登録を行うため、ユーザ認証を行います。テストを行なっているMACにはTouchIDがあるため、それを用いて認証します

  <p><img src="../images/ciam-vanilla-passkey-auth-01-app04.png" width="200"/></p>

- 登録が完了します

#### ログイン

- Username入力欄をクリックすると、Passkeyの選択メニューが表示されます。表示された中から、先ほど登録したアカウント名をクリックしてください

  <p><img src="../images/ciam-vanilla-passkey-auth-01-app05.png" width="400"/></p>

- TouchIDの操作が求められますので、こちらで認証します

  <p><img src="../images/ciam-vanilla-passkey-auth-01-app06.png" width="200"/></p>

- 正しくログインすることができました。この結果より、こちらのサイトに対してPasskeyの登録が完了し、正しくログインできたことがわかります

  <p><img src="../images/ciam-vanilla-passkey-auth-01-app07.png" width="400"/></p>

### 参考: 異なるデバイスのPasskeyを用いたログイン
- 同サイトに対し、異なる端末でPasskeyのログインができるようになっている場合、ログインの際に異なるデバイスを利用することが可能です

- ログイン画面で`Use a Different Passkey`をクリックします

  <p><img src="../images/ciam-vanilla-passkey-auth-01-another-device01.png" width="400"/></p>

  <p><img src="../images/ciam-vanilla-passkey-auth-01-another-device02.png" width="200"/></p>

- QRコードが表示されるので、すでにPasskeyの登録を行なっている別の端末で読み込みます

  <p><img src="../images/ciam-vanilla-passkey-auth-01-another-device03.png" width="200"/></p>

- アプリケーションは別端末の操作の完了を待ちます

  <p><img src="../images/ciam-vanilla-passkey-auth-01-another-device04.png" width="200"/></p>

- (モバイル端末)QRコードを読み込んだ端末でログインの操作を進めます

  <p><img src="../images/ciam-vanilla-passkey-auth-01-another-device05.png" width="200"/></p>

- 端末でログイン操作が完了すると、アプリケーションがログイン完了となります

### 参考: Chrome Passkey 管理画面

- `Google パスワード マネージャー` を開きます。左上のメニューから`設定`を開き、画面中断の`パスキーを管理`をクリックします
- 右上のテキストボックスにサイト名入力し、対象のサイトを検索できます
- 対象のPasskey右側`：`からPasskeyの削除が可能です

  <p><img src="../images/ciam-vanilla-passkey-auth-01-chrome-manage-passkey01.png" width="400"/></p>

  <p><img src="../images/ciam-vanilla-passkey-auth-01-chrome-manage-passkey02.png" width="200"/></p>

### 参考: iCloud Passkey 管理画面

- Apple端末で、`設定`の`パスワード`を開きます
- 画面上部のテキストボックスにサイト名を入力し、対象のサイトを検索できます
- 対象のPasskey右側`(i)`からエントリの詳細を確認し、最下部の`Delete`からPasskeyの削除が可能です

  <p><img src="../images/ciam-vanilla-passkey-auth-01-icloud-manage-passkey01.png" width="400"/></p>

  <p><img src="../images/ciam-vanilla-passkey-auth-01-icloud-manage-passkey02.png" width="200"/></p>



<!--
## デバッグ
-->

## 参考情報
- [WebAuthn quick start: Web SDK](https://developer.transmitsecurity.com/guides/webauthn/quick_start_sdk/)




