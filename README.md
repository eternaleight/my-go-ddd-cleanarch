## CtoC開発スタートパック (スケーラブル版)

最初に「CtoC開発スタートパック (初期高速開発版)」で開発を開始し、
\
プロジェクトの規模が大きくなったら「CtoC開発スタートパック (スケーラブル版)」の構成に移行するのも効率的です。

CtoC開発スタートパック(初期高速開発版)

ctoc-smart-dir
\
https://github.com/eternaleight/ctoc-smart-dir
### CtoC-Go-DDD-CleanArchitecture

```
クリーンアーキテクチャの依存性逆転の原則と依存性の注入を取り入れ、
依存関係を逆転させたレイヤードアーキテクチャにドメイン駆動設計の概念（DDD）を統合したもの。

　　　  -----------------------------
       |    Presentation Layer     |    ---> ユーザーや外部システムとのインターフェースを管理
       |-------------------------- |
       |   interfaces/router.go    |    ---> ルーティング設定、ミドルウェア処理、CORS設定
       -----------------------------　　　　　 main.go(ProvideHandlers)でhandlers、usecases、storesの依存性の注入
                    |
                    V
　　　  -----------------------------
       |    Interface Adapters     |    ---> プレゼンテーション層とアプリケーション層の間の橋渡し
       |-------------------------- |    ---> リクエストやレスポンスの形式変換
       |  interfaces/api/handlers  |    ---> APIエンドポイントのリクエスト処理
       | interfaces/api/middlewares|    ---> 認証や認可のミドルウェア処理
       -----------------------------
                    |
                    V
　　　  -----------------------------
       |    Application Layer      |    ---> ビジネスロジックを含むユースケースを管理
       |---------------------------|    ---> ドメインロジックの実行を制御
       |       app/usecases        |    ---> 認証、商品管理、ユーザー管理などのユースケース
       -----------------------------
                    |
                    V
　　　  -----------------------------
       |       Domain Layer        |    ---> ビジネスルールを表現する層（独立したモデル）
       |(Enterprise Business Rules)|    ---> ビジネスルールの定義
       |---------------------------|
       |       domain/models       |    ---> 商品、ユーザー、注文などのドメインモデル定義
       |       domain/rules        |    ---> ビジネスルール、バリデーションロジック
       -----------------------------
                    ^
                    |
　　　  -----------------------------
       |   Infrastructure Layer    |    ---> データベースや外部システムとのやり取りを管理
       |      (Data Access)        |    ---> 永続化、外部リソースとの通信を抽象化
       |---------------------------|
       |       infra/stores        |    ---> データベースのリポジトリ、ストアの実装
       -----------------------------


```

### Project structure

```
.
├── README.md                      // プロジェクトの基本情報、セットアップ手順、使用方法、依存関係などを記載したファイル
├── app                            // アプリケーション層
│   └── usecases                   // ユースケース（アプリケーション固有のビジネスロジックを実装）
│       ├── auth_usecase.go        // 認証に関するユースケース（例：ユーザー登録、ログイン、ログアウトのロジック）
│       ├── post_usecase.go        // 投稿に関するユースケース（例：新規投稿、投稿の編集、投稿の削除のロジック）
│       ├── product_usecase.go     // 商品に関するユースケース（例：商品登録、商品情報の更新、商品削除のロジック）
│       ├── purchase_usecase.go    // 購入に関するユースケース（例：購入手続き、購入履歴の確認のロジック）
│       └── user_usecase.go        // ユーザーに関するユースケース（例：ユーザー情報の取得、更新、削除のロジック）
├── config                         // 設定関連のファイルを格納
│   ├── config.go                  // アプリケーションの設定ファイル（例：環境変数の読み込み、設定値の管理）
│   └── database.go                // データベースの設定ファイル（例：データベース接続の初期化、接続設定）
├── domain                         // ドメイン層（ビジネスルールやドメインモデルを定義）
│   ├── models                     // ドメインモデル（データ構造を定義）
│   │   ├── post_model.go          // 投稿モデルの定義ファイル
│   │   ├── product_model.go       // 商品モデルの定義ファイル
│   │   ├── purchase_model.go      // 購入モデルの定義ファイル
│   │   └── user_model.go          // ユーザーモデルの定義ファイル
│   └── rules                     // ドメインルール（ビジネスルールを定義）
│       ├── auth_rule.go          // 認証に関するルール（例：パスワードのバリデーション、認証トークンの生成）
│       ├── post_rule.go          // 投稿に関するルール（例：投稿内容のバリデーション、公開範囲のチェック）
│       ├── product_rule.go       // 商品に関するルール（例：商品名のバリデーション、在庫管理のルール）
│       ├── purchase_rule.go      // 購入に関するルール（例：購入可能な商品のチェック、決済処理のルール）
│       └── user_rule.go          // ユーザーに関するルール（例：ユーザー名のバリデーション、メールアドレスの確認）
├── go.mod                         // Goモジュール設定ファイル（プロジェクトの依存関係を管理）
├── go.sum                         // Goモジュールの依存関係チェックサムファイル（依存関係のバージョン情報を保持）
├── infra                          // インフラ層（データベースや外部システムとのやり取りを管理）
│   └── stores                    // データストア（データベースアクセスロジックを格納）
│       ├── auth_store.go          // 認証データストア（ユーザー認証情報の保存、取得を行う）
│       ├── post_store.go          // 投稿データストア（投稿データの保存、取得を行う）
│       ├── product_store.go       // 商品データストア（商品データの保存、取得を行う）
│       ├── purchase_store.go      // 購入データストア（購入データの保存、取得を行う）
│       └── user_store.go          // ユーザーデータストア（ユーザーデータの保存、取得を行う）
├── interfaces                     // インターフェース層（プレゼンテーション層、ユーザーや他システムとのインターフェースを提供）
│   ├── api                       // APIインターフェース
│   │   ├── handlers              // ハンドラ（HTTPリクエストを処理する）
│   │   │   ├── auth.go            // 認証ハンドラ（ユーザー認証に関するリクエストを処理）
│   │   │   ├── post.go           // 投稿ハンドラ（投稿に関するリクエストを処理）
│   │   │   ├── product.go        // 商品ハンドラ（商品に関するリクエストを処理）
│   │   │   ├── purchase.go        // 購入ハンドラ（購入に関するリクエストを処理）
│   │   │   └── user.go            // ユーザーハンドラ（ユーザー情報に関するリクエストを処理）
│   │   ├── middlewares           // ミドルウェア（リクエストやレスポンスの前後で実行される処理を格納）
│   │   │   └── isAuthenticated.go // 認証ミドルウェア（認証状態の確認と処理を行う）
│   │   └── responses             // レスポンス（APIからの応答を生成するヘルパー関数）
│   │       ├── error.go           // エラーレスポンス（エラー応答の生成を行う）
│   │       └── success.go         // 成功レスポンス（成功応答の生成を行う）
│   └── router.go                 // ルータ設定（ルーティングの設定を行うファイル）
└── main.go                       // エントリーポイント（アプリケーションの開始点、サーバーの設定や初期化を含む）
```
