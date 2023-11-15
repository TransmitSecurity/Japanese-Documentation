
# Hosted IDV(ID Verification)

## 対象サンプルアプリケーション
- [hosted-idv](https://github.com/TransmitSecurity/ciam-expressjs-vanilla-samples/tree/main/hosted-idv)
## 処理フロー

### ログイン時の処理フロー

こちらのログインフローは、デスクトップにてブラウザ(Client)で操作を開始する想定となります。
IDVはアカウントの生成などは行わず、以下のフローに従って利用をする人が正しいかどうかを評価します。

```mermaid
sequenceDiagram
    participant User
    participant Client as Client (Browser)
    participant Mobile as Mobile Device
    participant Backend
    participant Transmit Security

    User->>Client: 1. Root(/)へ接続
    Client->>Backend: 
    Backend->>Client: /pages/hosted-idv-experience.html へHTTP Redirect
    User->>Client: 2. ボタン(Start IDV Flow)をクリック
    Client->>Backend: 3. IDVリクエストを送信(GET /start-verification-session)
    Backend->>Transmit Security: 4. AccessToken取得(POST /oidc/token)
    Transmit Security->>Backend: AccessTokenを応答
    Backend->>Transmit Security: 5. IDVのセッション作成 (POST /verify/api/v1/verification)
    Transmit Security->>Backend: IDVのセッション作成結果を応答
    Backend->>Client: 6. IDVの初期化。Redirect URLをClientに応答(/verify/app/<IDVのセッション作成結果のstart_tokenの値>)
    Client->>Transmit Security: 7. 送付されたRedirect URLに接続(GET /verify/app/<start_token>)
    Transmit Security->>Client: 8. IDVのためQRコード・URLを表示

    Client --> Transmit Security: MobileによるIDV完了まで定期的にStatusチェック


    opt MobileによるIDVの実施
      User ->> Mobile: 画面に表示されたQRコードを読み取り
      Mobile ->> Transmit Security: Hosted IDVの実施
      Mobile --> Transmit Security: 身分証の撮影
      Mobile --> Transmit Security: セルフィの撮影
      Transmit Security ->> Mobile: 完了画面の表示
    end 

    Client --> Transmit Security: Statusチェックの結果、IDV完了を検知
    Client->>Backend: 9. IDV結果の取得。/complete へ接続
    Backend->>Transmit Security: 10. AccessToken取得(POST /oidc/token)
    Transmit Security->>Backend: AccessTokenを応答
    Backend->>Transmit Security: 11. IDVの結果取得 (POST /verify/api/v1/verification/<sessionId>/result)
    Transmit Security->>Backend: IDV結果の応答
    Backend->>Backend: recommendationの結果を評価。評価結果に応じたHTMLを生成
    Backend->>Client: 生成したHTMLを応答

```

- 5の応答結果として以下のような値が応答されます
  ```json
  {
    // Use the start token to start the verification on the client side
    "start_token": "abcd6ed78c8c0b7824dfea356ed30b72",
    "session_id": "EXAMPLEskjzsdhskj4",
    "expiration": "2023-12-27T11:59:07.420Z",
    // Array of images to capture
    "missing_images": [
        "document_front",
        "document_back",
        "selfie"
    ]
  }
  ```

- 6で生成するURLは5のstart_tokenを利用します
- 9は5のsession_idをURLパラメータとして渡します
- 11でsession_idを利用してAPIに接続します


### 利用するTransmit SecurityのAPI

  | STEP | 役割 | API | 
  | --- | --- | --- |
  |4,10|APIに利用するAccessTokenの取得|[Get client access token](https://developer.transmitsecurity.com/openapi/token/#operation/getAccessToken)|
  |5|IDVセッションの作成|[Create verification session](https://developer.transmitsecurity.com/openapi/verify/verifications/#operation/createSession)|
  |11|IDVの結果取得|[Get verification result](https://developer.transmitsecurity.com/openapi/verify/verifications/#operation/getResult)|


## はじめに
- 本ドキュメントではサンプルアプリケーションの利用に関する手順を示します
- サンプルアプリケーションを[ローカル環境で実行](./setup.md#ローカル環境で実行)した際の手順を示しています。試される環境に合わせて適宜アクセスするURLなど変更して操作ください

### 事前準備・前提
- 本ドキュメントでは以下が必要となります
  - インターネットに接続可能な端末
  - ブラウザ
  - 送受信可能なメールアドレス
  - 手順に応じた簡易なCLI操作・ファイル編集
- 各国でサポートされているドキュメントの情報は[Supported documents](https://developer.transmitsecurity.com/guides/verify/global_coverage/)を参照してください


## サンプルアプリケーションの実行
```
SAMPLE=hosted-idv yarn start
```

## 動作確認

### アプリケーション利用手順

- ブラウザでサンプルアプリケーション([http://localhost:8080](http://localhost:8080))に接続

#### IDVの実施
- アプリケーションの画面に表示されたボタンをクリック

  <img src="../images/ciam-vanilla-hosted-idv-01-app01.png" width="400"/>

- IDVを開始するため、QRコード・URLが表示されます。モバイル端末で情報を読み取り、IDVのセッションを開始します

  <img src="../images/ciam-vanilla-hosted-idv-01-app02.png" width="400"/>

- モバイル端末で指示に従って操作を行います。このサンプルでは`パスポート`を利用しIDVを実施しています。IDVではカメラを利用しますので、適切な手順で端末・ブラウザでカメラの利用を許可してください

  - IDVを開始します

  <p><img src="../images/ciam-vanilla-hosted-idv-01-mobile01.png" width="200"/></p>

  - パスポートの写真が記載されているページの情報を読み取ります

  <p><img src="../images/ciam-vanilla-hosted-idv-01-mobile02.png" width="200"/></p>

  - セルフィー（インカメラによる顔写真）を撮影します

  <p><img src="../images/ciam-vanilla-hosted-idv-01-mobile03.png" width="200"/></p>

  - 正しい情報であるか評価します。一定時間経過後、結果が画面に表示されます

  <p><img src="../images/ciam-vanilla-hosted-idv-01-mobile04.png" width="200"/></p>

  <p><img src="../images/ciam-vanilla-hosted-idv-01-mobile05.png" width="200"/></p>


- 上記のモバイル端末での操作が完了すると、自動的にデスクトップのブラウザに評価結果が表示されます

  <img src="../images/ciam-vanilla-hosted-idv-01-app03.png" width="400"/>

#### Portalの確認

- Identitiy Verifification > Verifications に評価結果が表示されます

  <img src="../images/ciam-vanilla-hosted-idv-01-portal01.png" width="400"/>

- こちらはStatusでフィルタされています。フィルタは以下の中からStatusを選択できます

  <img src="../images/ciam-vanilla-hosted-idv-01-portal02.png" width="400"/>

- エントリをクリックすると、評価の詳細を確認できます

  <img src="../images/ciam-vanilla-hosted-idv-01-portal03.png" width="400"/>

- RiskScoreをクリックすると、Recommendationsを開き、IDV評価がどのように判定されたか詳細を確認することが可能です

  <p><img src="../images/ciam-vanilla-hosted-idv-01-portal04.png" width="400"/></p>

  - 対象となるエントリをクリックさらに情報の閲覧が可能です

  <p><img src="../images/ciam-vanilla-hosted-idv-01-portal05.png" width="400"/></p>

  - 対象エントリ右上の「:」をクリックし、Raw Dataを選択することで、IDVの詳細や判定に関する情報を確認できます

  <p><img src="../images/ciam-vanilla-hosted-idv-01-portal06.png" width="400"/></p>


<!--
### Developer Portal ステータス

> **Warning**
> Portalの機能、画面のデザインは日々アップデートされます。本ページの画像は参考情報としてご確認ください

## デバッグ
-->

## 参考情報
- [Web hosted IDV](https://developer.transmitsecurity.com/guides/verify/quick_start_web/)