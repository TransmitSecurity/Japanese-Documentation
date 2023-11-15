
# Backend SMS OTP Authentication

## 対象サンプルアプリケーション
- [login-with-be-sms](https://github.com/TransmitSecurity/ciam-expressjs-vanilla-samples/tree/main/login-with-be-sms)

## 処理フロー

### ログイン時の処理フロー

```mermaid
sequenceDiagram
    participant User
    participant Client as Client (Browser)
    participant Backend
    participant Transmit Security

    opt ユーザを新規作成する場合
      User->>Client: 1. Root(/)へ接続
      Client->>Backend: 
      Backend->>Client: /pages/signup.html へHTTP Redirect
      User->>Client: 2. 有効な電話番号を入力
      Client->>Backend: 3. ユーザ(電話番号)を新規作成 (POST /create-user)
      Backend->>Transmit Security: 4. AccessToken取得(POST /oidc/token)
      Transmit Security->>Backend: AccessTokenを応答
      Backend->>Transmit Security: 5. ユーザの新規作成 (POST /users)
      Transmit Security->>Backend: 新規作成結果送信
      Backend->>Client: 新規作成結果送信
    end
    User->>Client: 6. Login(/pages/sms-otp.html)へ接続
    User->>Client: 7. 有効な電話番号を入力
    Client->>Backend: 8. 有効な電話番号へSMSでOTP送信(POST /send-sms-otp)
    Backend->>Transmit Security: 9. AccessToken取得(POST /oidc/token)
    Transmit Security->>Backend: AccessTokenを応答
    Backend->>Transmit Security: 10. OTP送信 (POST /auth/otp/send)
    Transmit Security->>Backend: OTP送信結果
    Backend->>Client: SMS OTP送信結果を表示
    Transmit Security-->>User: 11. OTPをSMSで送信
    User->>Client: 12. OTPを入力
    Client->>Backend: 13. SMS OTP送信 (POST /authenticate)
    Backend->>Transmit Security: 14. OTP評価 (POST /auth/otp/authenticate)
    Transmit Security->>Backend: OTP評価結果・User Access Tokenの応答
    opt User Access Tokenの検証
      Backend->>Transmit Security: 15. JWKSの取得 (GET /oidc/jwks)
      Transmit Security->>Backend: 取得したJWKSでUser Access Tokenを検証
    end
    Backend->>Client: OTP評価結果を表示
    Client->>User: 16. OTP評価結果を表示
```
- 14以前(4,9)でAccess Token取得済みのため、14ではそのAccessTokenを利用するため新規取得を行いません


### 利用するTransmit SecurityのAPI

  | STEP | 役割 | API | 
  | --- | --- | --- |
  |4、9|APIに利用するAccessTokenの取得|[Get client access token](https://developer.transmitsecurity.com/openapi/token/#operation/getAccessToken)|
  |5|ユーザの作成|[Create User](https://developer.transmitsecurity.com/openapi/user/user/#operation/createUser)|
  |10|対象SMSへOTPの送信|[Backend Send OTP](https://developer.transmitsecurity.com/openapi/user/backend-one-time-login/#operation/sendOTP)|
  |14|OTPの認証|[Backend Authenticate OTP](https://developer.transmitsecurity.com/openapi/user/backend-one-time-login/#operation/authenticateOTP)|



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
SAMPLE=login-with-be-sms yarn start
```


### アプリケーション利用手順
- ブラウザでサンプルアプリケーション([http://localhost:8080](http://localhost:8080))に接続

#### ユーザー新規登録
- 初期画面がユーザー新規作成画面となります
   - `作成済みのユーザーでログインする場合には`画面下部の`Login`をクリックし、以降は[SMS OTP](./login-with-be-sms.md#sms-otp)を参照してください
   - アプリケーションの画面に、`(Platform上にアカウントが作成されていない)有効な電話番号`を入力し、下に表示されたボタンをクリック

  <img src="../images/ciam-vanilla-login-with-be-sms-01-app01.png" width="400"/>

- ユーザ作成が完了し、以下の画面が表示されます

  <img src="../images/ciam-vanilla-login-with-be-sms-01-app02.png" width="400"/>

#### SMS OTP

- ユーザ作成完了の`Login`、または初期画面の`Login`ボタンをクリックすると、メールアドレス入力画面が表示されます。`有効な電話番号`を入力し、ボタンをクリックしてください

  <img src="../images/ciam-vanilla-login-with-be-sms-01-app03.png" width="400"/>

- `有効な電話番号`にSMSが届きます。通知された`OTP`の内容を確認します

  <img src="../images/ciam-vanilla-login-with-sms-01-sms01.png" width="400"/>

- アプリケーションの画面がOTP入力画面に遷移していることを確認し、先ほどメールで確認した`OTP`を入力します

  <img src="../images/ciam-vanilla-login-with-sms-01-app02.png" width="400"/>

- `OTP`が正しいことが確認でき、画面が遷移します

  <img src="../images/ciam-vanilla-login-with-be-sms-01-app04.png" width="400"/>

<!--
### Developer Portal ステータス

> **Warning**
> Portalの機能、画面のデザインは日々アップデートされます。本ページの画像は参考情報としてご確認ください

## デバッグ
-->

## 参考情報
- [Backend Authentication/Login with SMS OTP](https://developer.transmitsecurity.com/guides/user/be_auth_sms_otp/)