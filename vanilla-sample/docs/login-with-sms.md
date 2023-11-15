
# SMS OTP Authentication

## 対象サンプルアプリケーション
- [login-with-sms](https://github.com/TransmitSecurity/ciam-expressjs-vanilla-samples/tree/main/login-with-sms)

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
    Backend->>Client: /pages/sms-otp.html へHTTP Redirect
    User->>Client: 2. 有効な電話番号を入力
    Client->>Backend: 3. 有効な電話番号へSMSでOTP送信(POST /sms-otp)
    Backend->>Transmit Security: 4. AccessToken取得(POST /oidc/token)
    Transmit Security->>Backend: AccessTokenを応答
    Backend->>Transmit Security: 5. Email OTP送信 (POST /auth/otp/sms)
    Transmit Security->>Backend: OTP送信結果
    Backend->>Client: OTP送信結果を表示
    Transmit Security-->>User: 6. OTPをSMSで送信
    User->>Client: 7. OTPを入力
    Client->>Backend: 8. OTP送信 (POST /verify)
    Backend->>Transmit Security: 9. SMS OTP評価 (POST /auth/otp/sms/validation)
    Transmit Security->>Backend: SMS OTP評価後に接続するURLを応答(/cis/oidc/auth?)
    Backend->>Client: SMS OTP評価後に接続するURLを応答(/cis/oidc/auth?)
    Client->> Transmit Security: 10. SMS OTP結果で返されたURLに接続(GET /cis/oidc/auth?)
    Transmit Security ->> Client: ClientのRedirect URL(GET /complete)へHTTP Redirect 
    Client->>Backend: /complete へ接続
    Backend->>Client: /pages/complete.htmlへHTTP Redirect
    Client->>User: 11. complete.htmlを表示
```

- 9以前(4)でAccess Token取得済みのため、9ではそのAccessTokenを利用するため新規取得を行いません
- 5でAPIに接続する際のオプションパラメータに[create_new_user: true](https://developer.transmitsecurity.com/openapi/user/one-time-login/#operation/sendSmsOtp!path=create_new_user&t=request)を指定しています。この値を指定することにより、対象のユーザがTransmitSecurityのテナントに存在しない場合、新規ユーザとして生成されます

### 利用するTransmit SecurityのAPI

  | STEP | 役割 | API | 
  | --- | --- | --- |
  |4|APIに利用するAccessTokenの取得|[Get client access token](https://developer.transmitsecurity.com/openapi/token/#operation/getAccessToken)|
  |5|対象電話番号へSMSを使ったOTPの送信|[Send SMS OTP](https://developer.transmitsecurity.com/openapi/user/one-time-login/#operation/sendSmsOtp)|
  |9|SMS OTPの評価|[Validate SMS OTP](https://developer.transmitsecurity.com/openapi/user/one-time-login/#operation/validateSms)|
  |10|OIDCによる認可|[Authorization](https://developer.transmitsecurity.com/openapi/user/oidc/#operation/oidcAuthenticate)|


## はじめに
- 本ドキュメントではサンプルアプリケーションの利用に関する手順を示します
- サンプルアプリケーションを[ローカル環境で実行](./setup.md#ローカル環境で実行)した際の手順を示しています。試される環境に合わせて適宜アクセスするURLなど変更して操作ください
- SMSでOTPを通知します。あらかじめSMS設定を完了してください

### 事前準備・前提
- 本ドキュメントでは以下が必要となります
  - インターネットに接続可能な端末
  - ブラウザ
  - 送受信可能な電話番号(SMS)
  - 手順に応じた簡易なCLI操作・ファイル編集

## サンプルアプリケーションの実行
```
SAMPLE=login-with-sms yarn start
```

## 動作確認

### アプリケーション利用手順
- ブラウザでサンプルアプリケーション([http://localhost:8080](http://localhost:8080))に接続

- アプリケーションの画面に、`有効な電話番号`を入力し、下に表示されたボタンをクリックします。電話番号は`+<国番号><電話番号>(ex: +819011112222)`で入力してください

  <img src="../images/ciam-vanilla-login-with-sms-01-app01.png" width="400"/>

- `有効な電話番号`にSMSが届きます。通知された`OTP`の内容を確認します

  <img src="../images/ciam-vanilla-login-with-sms-01-sms01.png" width="400"/>

- アプリケーションの画面がOTP入力画面に遷移していることを確認し、先ほどメールで確認した`OTP`を入力します

  <img src="../images/ciam-vanilla-login-with-sms-01-app02.png" width="400"/>

- `OTP`が正しいことが確認でき、画面が遷移します

  <img src="../images/ciam-vanilla-login-with-sms-01-app-login01.png" width="400"/>


<!--
### Developer Portal ステータス

> **Warning**
> Portalの機能、画面のデザインは日々アップデートされます。本ページの画像は参考情報としてご確認ください

## デバッグ
-->

## 参考情報
- [Login with SMS OTP](https://developer.transmitsecurity.com/guides/user/auth_sms_otp/)