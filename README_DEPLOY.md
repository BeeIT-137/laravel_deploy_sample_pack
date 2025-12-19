# Laravel Deploy Sample Pack (public_html)

Gói này KHÔNG chứa full Laravel framework (vendor + core) vì bạn cần tạo project Laravel bằng Composer.
Gói này cung cấp:
- File mẫu để chạy Laravel khi DocumentRoot là public_html
- Route + view test (không DB)
- GitHub Actions deploy (FTP / SSH)
- Quy tắc bảo mật (.htaccess) cho shared hosting

## 1) Tạo project Laravel ở máy bạn
```bash
composer create-project laravel/laravel laravel-test
cd laravel-test
```

## 2) Patch các file test (không DB)
Copy:
- laravel_patch_files/routes/web.php  -> routes/web.php
- laravel_patch_files/resources/views/welcome-test.blade.php -> resources/views/welcome-test.blade.php

## 3) Chuẩn bị cấu trúc upload lên host (public_html)
Trên host tạo:
- public_html/laravel_app  (chứa toàn bộ source Laravel: app/, bootstrap/, config/, routes/, resources/, vendor/, storage/, ...)

Sau đó copy 2 file sau ra public_html:
- host_layout/public_html/index.php   -> public_html/index.php
- host_layout/public_html/.htaccess   -> public_html/.htaccess

**Quan trọng:** public_html/index.php đã trỏ vào public_html/laravel_app/...

## 4) .env trên host (không DB)
Tạo file: public_html/laravel_app/.env
Lấy mẫu từ: laravel_patch_files/.env.example
Nhớ có APP_KEY. Tạo ở local:
```bash
php artisan key:generate
```
Rồi copy APP_KEY sang host.

## 5) Quyền thư mục
Cần writable:
- public_html/laravel_app/storage
- public_html/laravel_app/bootstrap/cache

Nếu có SSH:
```bash
chmod -R 775 storage bootstrap/cache
```

## 6) Deploy bằng GitHub Actions
Xem các workflow mẫu ở:
- .github/workflows/deploy_ftp.yml
- .github/workflows/deploy_ssh.yml

Bạn cần điền Secrets trong GitHub repo.
