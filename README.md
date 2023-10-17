# railsSampleApp
これは、ReactとRuby on railsでのアプリケーション開発に使用することのできるサンプルdoker構成です。

これで構築できます。

## 構築方法

### Backend

①ファイルをダウンロード

②dockerを起動し、下記コマンドを/railsSampleApp直下で実行する

```bash
docker compose run backend rails new . --force --database=mysql --api
```

③イメージビルドを行う

```bash
docker compose build
```

④config/database.ymlに下記項目を記載

```
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password
  host: db
```

⑤コンテナを起動してdbを作成する

```
docker compose up -d
docker compose exec api rails db:create
```

[http://localhost:3001](http://localhost:3001/)でアクセス可能

### Frontend

①下記コマンドでfrontコンテナに入る

```
docker compose exec front bash
```

②frontコンテナに入ったあとreactプロジェクトを作成

```
npx create-react-app . --template typescript
```

③npm startで起動

```
npm start
```

[http://localhost:3000](http://localhost:3000/)でアクセス可能

