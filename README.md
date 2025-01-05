# errata

本リポジトリでは、書籍「RustによるWebアプリケーション開発　設計からリリース・運用まで」（以下、本書）の正誤情報を管理します。

本書についてお気づきの点がある場合、本リポジトリのIssueにてお知らせください。確認の上、以下の正誤表に反映いたします。

## 正誤表

### 第1章

#### P.2 「1.2.1 ユーザーの管理」７行目以下の分類の列挙

最下部の「ユーザーの一覧の閲覧」のインデントが1つ足りませんでした。

誤:

> - ユーザー全員が行える操作
>   - 自身に対する操作
>     - ログイン
>     - ログアウト
>     - ユーザー情報の表示
>     - パスワード変更
> - ユーザーの一覧の閲覧

正:

> - ユーザー全員が行える操作
>   - 自身に対する操作
>     - ログイン
>     - ログアウト
>     - ユーザー情報の表示
>     - パスワード変更
>   - ユーザーの一覧の閲覧

### 第2章

#### P.27 「2.4.2 リポジトリのクローン」に記載のURL

誤: https://github.com/rust-web-app-book/rusty-book-manager
正: https://github.com/rust-web-app-book/rusty-book-manager-template


### 第5章

#### P.134 「src/bin/app.rs：リクエスト・レスポンス時のログ出力の設定」コードブロック内の13行目付近

誤:

```rust
    let app = Router::new()
        .merge(v1::routes())
        .merge(auth::routes())
        .layer(cors())
```

正:

```rust
    let app = Router::new()
        .merge(build_health_check_routers())
        .merge(build_book_routers())
```

#### P.163〜164 「src/bin/app.rs：Redis接続の初期化処理」内のコード

P.134で追加したログ出力のレイヤーがコードから抜けていました。

誤:

```rust
    let app = Router::new()
        .merge(v1::routes())
        .merge(auth::routes())
        .with_state(registry);
```

正:

```rust
    let app = Router::new()
        .merge(v1::routes())
        .merge(auth::routes())
        .layer(
            TraceLayer::new_for_http()
                .make_span_with(DefaultMakeSpan::new().level(Level::INFO))
                .on_request(DefaultOnRequest::new().level(Level::INFO))
                .on_response(
                    DefaultOnResponse::new()
                        .level(Level::INFO)
                        .latency_unit(LatencyUnit::Millis),
                ),
        )
        .with_state(registry);
```

### 第6章

#### P.245 本文3行目 および 本文5行目

誤: （例では `fn test_func`）
正: （例では `fn 任意の関数名`）

