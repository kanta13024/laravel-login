# laravel-login
ログイン機能の実装（侍　５週目）

⓪データベースの作成
DBever 接続→外部接続のためのUser作成
root userで
mysql> SELECT user, host FROM mysql.user ORDER BY user; →ユーザー一覧の表示（グローバルカードの確認）
mysql> GRANT [権限] ON [適用対象のデータベース].[適用対象のテーブル] TO 'ユーザ名'@'ホスト名' IDENTIFIED BY 'パスワード';　→userの権限の変更
権限：ALL
適用範囲：*.*
例
GRANT ALL ON *.* TO 'phper'@'%';
mysql> FLUSH PRIVILEGES; →変更確定のコマンド
mysql> SHOW GRANTS FOR 'ユーザ名'@'ホスト名'; →権限変更の確認

DBever or mysql でのデータベースの作成
CREATE DATABASE laravel_login

・migrations ファイルの作成
<img width="1189" alt="スクリーンショット 2021-05-08 17 42 33" src="https://user-images.githubusercontent.com/82421244/117532992-13f47b00-b025-11eb-9c21-da65336679cd.png">


php artisan migrate　の実行
※php artisan migrate:refresh 全てのmigrationをロールバックしてmigration実行
※DBever等でデータベースにマイグレーションできたか確認

<img width="932" alt="スクリーンショット 2021-05-09 8 39 49" src="https://user-images.githubusercontent.com/82421244/117557796-cbcf6a00-b0b1-11eb-9932-68285d664f05.png">
<img width="1440" alt="スクリーンショット 2021-05-09 8 39 57" src="https://user-images.githubusercontent.com/82421244/117557797-ceca5a80-b0b1-11eb-853e-6a3f933d0eed.png">
<img width="949" alt="スクリーンショット 2021-05-09 8 40 02" src="https://user-images.githubusercontent.com/82421244/117557799-d1c54b00-b0b1-11eb-9a12-41c9ce808793.png">
<img width="912" alt="スクリーンショット 2021-05-09 8 40 20" src="https://user-images.githubusercontent.com/82421244/117557801-d4c03b80-b0b1-11eb-85b4-1c439876a149.png">
<img width="1104" alt="スクリーンショット 2021-05-09 8 44 15" src="https://user-images.githubusercontent.com/82421244/117557803-d7bb2c00-b0b1-11eb-8f03-ec2643a236aa.png">

app/Models/User.php　の編集
<img width="1158" alt="スクリーンショット 2021-05-09 8 57 58" src="https://user-images.githubusercontent.com/82421244/117557805-dbe74980-b0b1-11eb-99d1-0b2314eab6ce.png">

database/factory/UserFactory.phpの編集

database/seeders/DatabaseSeeder.phpの編集
<img width="1136" alt="スクリーンショット 2021-05-09 10 39 52" src="https://user-images.githubusercontent.com/82421244/117557910-ff5ec400-b0b2-11eb-9d3b-c60938c243ac.png">
シードのコマンド実行 //ダミーデータの作成
php artisan db:seed

config/app.php
facker_locale -> 'ja_JP',
php artisan migrate:refresh --seed

①Router設定
<img width="1080" alt="スクリーンショット 2021-05-08 16 42 53" src="https://user-images.githubusercontent.com/82421244/117531231-86149200-b01c-11eb-8ed1-911dc153614e.png">

web.php

Route::get('/', [AuthController::class, 'showLogin'])->name('showLogin');
Route::post('login', [AuthController::class, 'login'])->name('login');
の追加

②Controller設定
③ViewとBootstrapの設定
・Controller ディレクトリ、ファイルの作成
php artisan make:controller Auth/AuthController

・AuthController.phpの作成
<img width="783" alt="スクリーンショット 2021-05-08 16 55 21" src="https://user-images.githubusercontent.com/82421244/117531605-38008e00-b01e-11eb-99c9-e231993d2116.png">

・resources/views/login/login_form.blade.phpの作成
bootstrapの導入
npm install && npm run dev
→public/css
→public/js　にbootstrapを追加
・bootstrapの適用
<img width="812" alt="スクリーンショット 2021-05-08 17 02 25" src="https://user-images.githubusercontent.com/82421244/117531812-7185c900-b01f-11eb-8773-28adfb365922.png">
※サンプルデータをダウンロードし、コピー＆ペースト
login_form.blade.php
public/css/signin.css
にペースト

method, action のルートの編集

④バリデーションの設定
php artisan make:request LoginFormRequest
→app/Http/Requests/LoginFormRequest.php
<img width="1198" alt="スクリーンショット 2021-05-08 17 19 33" src="https://user-images.githubusercontent.com/82421244/117532303-bf9bcc00-b021-11eb-8cca-ddf46823e8e0.png">

Http/Controller/Auth/AuthController.php
AuthController.php
<img width="997" alt="スクリーンショット 2021-05-08 17 25 17" src="https://user-images.githubusercontent.com/82421244/117532426-697b5880-b022-11eb-91dc-83353de7a572.png">

login_form.blade.php
バリデーションの設定
※laravel公式よりコピー＆ペースト
<img width="782" alt="スクリーンショット 2021-05-08 17 36 48" src="https://user-images.githubusercontent.com/82421244/117532754-1c988180-b024-11eb-9919-e60614d5c4ca.png">

resourdes/lang/ja
validation.php
バリデーションの日本語の設定
<img width="1189" alt="スクリーンショット 2021-05-08 17 42 33" src="https://user-images.githubusercontent.com/82421244/117532992-13f47b00-b025-11eb-9c21-da65336679cd.png">

⑤Authの確認

⑥ログイン後のページ作成

⑦ミドルウェアの設定

⑧ログアウト機能