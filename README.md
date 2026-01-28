# random-chat Laravel 12 專案模板

可直接啟動的 Laravel 12 + Inertia + Vue 3 專案模板，內建 Docker 開發環境與 Vite HMR 反代設定。

## 技術版本

- PHP: ^8.2
- Laravel Framework: ^12.0
- Inertia (Laravel): ^2.0
- Fortify: ^1.30
- Wayfinder: ^0.1.9
- Vite: ^7.0
- Vue: ^3.5
- Tailwind CSS: ^4.1
- MySQL: 8.0
- Redis: latest

## 環境變數設定（.env）

從 `.env.example` 複製成 `.env` 後，至少調整以下內容：

- `APP_URL`: 由 `http://localhost` 改成你的目標網域，例如 `http://your-domain.test`
- `VITE_HMR_HOST`: 改成你的目標網域（與 `APP_URL` 保持一致）
- `DB_DATABASE`: 你的資料庫名稱
- `DB_USERNAME` / `DB_PASSWORD`: 你的資料庫帳密

如果你會用 Cookie / Session 綁定網域，請視需求同步調整：

- `SESSION_DOMAIN`: 例如 `.your-domain.test` 或留空

## Nginx 網域設定（Docker）

若使用本專案內建的 Nginx 設定，請同步修改：

- `dockerfiles/nginx/conf.d/default.conf` 的 `server_name` 為你的目標網域

在本機開發時，記得把網域指到 127.0.0.1：

```
127.0.0.1 your-domain.test
```

## 啟動方式（Docker 開發環境）

1) 複製環境檔
```
cp .env.example .env
```

2) 依照「環境變數設定」與「Nginx 網域設定」調整網域與資料庫設定

3) 建立反向代理網路（第一次需要）
```
docker network create proxy-network
```

4) 啟動服務
```
docker compose up -d
```

5) 安裝依賴並初始化
```
docker compose exec web composer install
docker compose exec web php artisan key:generate
docker compose exec web php artisan migrate
```

## 啟動方式（非 Docker）

如果你要用本機環境啟動（需要 PHP、Composer、Node.js）：

```
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
npm install
npm run dev
```

## 其他注意事項

- Vite HMR 透過 Nginx 反代 `/vite`，如果熱更新無效，請確認 `VITE_HMR_HOST` 與 `server_name` 一致
- `dockerfiles/nginx/snippets/node.conf` 已包含 Vite 開發所需的 proxy 設定
