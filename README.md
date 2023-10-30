# railsSampleApp
これは、ReactとRuby on railsでのアプリケーション開発に使用することのできるサンプルdoker構成です。

これで構築できます。

## 構築方法

### Backend

①ファイルをダウンロード

②dockerを起動し、下記コマンドを/railsSampleApp/backed直下で実行する

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

### (nginxも起動させる場合)

config/puma.rbに下記コードを記載で実行可能

```
app_root = File.expand_path("../..", __FILE__)
bind "unix://#{app_root}/tmp/sockets/puma.sock"

stdout_redirect "#{app_root}/log/puma.stdout.log", "#{app_root}/log/puma.stderr.log", true
```

