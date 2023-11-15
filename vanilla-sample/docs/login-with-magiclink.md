
# Email Magiclink Authentication

## 対象サンプルアプリケーション
- [login-with-magiclink](https://github.com/TransmitSecurity/ciam-expressjs-vanilla-samples/tree/main/login-with-magiclink)

## 処理フロー

### ログイン時の処理フロー


```mermaid
sequenceDiagram
    participant User
    participant Client as Client (Browser)
    participant Backend
    participant Transmit Security

    User->>Client: 1. Root(/)へ接続
    Client->>Backend: 
    Backend->>Client: /pages/magiclink.html へHTTP Redirect
    User->>Client: 2. Email(demo@xxx.com)を入力
    Client->>Backend: 3. EmailへMagiclink送信(POST /send-magiclink)
    Backend->>Transmit Security: 4. AccessToken取得(POST /oidc/token)
    Transmit Security->>Backend: AccessTokenを応答
    Backend->>Transmit Security: 5. Email Magiclink送信 (POST /auth/links/email)
    Transmit Security->>Backend: Magiclink送信結果
    Backend->>Client: Magiclink送信結果を表示
    Transmit Security-->>User: 6. Magiclinkをメールで送信
    opt Magiclinkをクリックすると、新規タブで以下が実行される
      User->>Transmit Security: 7. メールを開き、Magiclinkをクリック (GET /cis/auth/email/clicked?)
      Transmit Security->>Client: Magiclinkの接続を評価後、OIDC認可のURLへHTTP Redirect(/cis/oidc/auth?)
      Client->> Transmit Security: OIDC認可のURLへ接続(GET /cis/oidc/auth?)
      Transmit Security ->> Client: ClientのRedirect URL(GET /complete)へHTTP Redirect 
      Client->>Backend: /complete へ接続
      Backend->>Client: /pages/complete.htmlへHTTP Redirect
      Client->>User: 9. complete.htmlを表示
    end
```

- 5でAPIに接続する際のオプションパラメータに[create_new_user: true](https://developer.transmitsecurity.com/openapi/user/one-time-login/#operation/SendEmail!path=create_new_user&t=request)を指定しています。この値を指定することにより、対象のユーザがTransmitSecurityのテナントに存在しない場合、新規ユーザとして生成されます


### 利用するTransmit SecurityのAPI

  | STEP | 役割 | API | 
  | --- | --- | --- |
  |4|APIに利用するAccessTokenの取得|[Get client access token](https://developer.transmitsecurity.com/openapi/token/#operation/getAccessToken)|
  |5|対象メールアドレスへMagiclinkの送信|[Send email link](https://developer.transmitsecurity.com/openapi/user/one-time-login/#operation/SendEmail)|


## はじめに
- 本ドキュメントではサンプルアプリケーションの利用に関する手順を示します
- サンプルアプリケーションを[ローカル環境で実行](./setup.md#ローカル環境で実行)した際の手順を示しています。試される環境に合わせて適宜アクセスするURLなど変更して操作ください
- EmailでMagiclinkを通知します。あらかじめEmail設定を完了してください

### 事前準備・前提
- 本ドキュメントでは以下が必要となります
  - インターネットに接続可能な端末
  - ブラウザ
  - 送受信可能なメールアドレス
  - 手順に応じた簡易なCLI操作・ファイル編集

## サンプルアプリケーションの実行
```
SAMPLE=login-with-magiclink yarn start
```

## 動作確認

### アプリケーション利用手順
- ブラウザでサンプルアプリケーション([http://localhost:8080](http://localhost:8080))に接続

- アプリケーションの画面に、`有効なメールアドレス`を入力し、下に表示されたボタンをクリック

  <img src="../images/ciam-vanilla-login-with-magiclink-01-app01.png" width="400"/>

- メールアドレスに対してMagiclinkを送信が完了した旨、メッセージが表示されます

  <img src="../images/ciam-vanilla-login-with-magiclink-01-app02.png" width="400"/>

- `有効なメールアドレス`の受信ボックスを開き、通知されたメール内の`Loginボタン(Magiclink)`をクリックします

  <img src="../images/ciam-vanilla-login-with-magiclink-01-mail01.png" width="400"/>

- Loginボタンクリック後、操作が正しいことが確認できた後、画面が遷移します

  <img src="../images/ciam-vanilla-login-with-magiclink-01-app-login01.png" width="400"/>

<!--
### Developer Portal ステータス

> **Warning**
> Portalの機能、画面のデザインは日々アップデートされます。本ページの画像は参考情報としてご確認ください

## デバッグ
-->

## 参考情報
- [Login with email magic links](https://developer.transmitsecurity.com/guides/user/auth_email_magic_link/)