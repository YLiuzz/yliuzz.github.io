---
title: laradock 多專案建置 x  laravel  admin 建置 (MAC)
date: 2021-12-02 11:06:28
categories: back-end
description: Laradock 是一個用 Docker 打造完成的 PHP 開發環境
tags:
- laradock
- 後端技術
---
某天突然看到這個工具，因此對這個工具產生好奇，所以就來研究看看拉，以下是我實作過後的結果。
[Laradock](https://laradock.io/) 是一個用 Docker 打造完成的 PHP 開發環境 ～
如果有小型專案可以使用Laradock 進行開發～
讓我們一起來學習如何使用Laradock這個工具吧！
<!-- more -->
### 下載docker
[Docker官網](https://www.docker.com/get-started)
###  Laradock官網
[Laradock](https://laradock.io/)
### 設定環境
需要工具 composer 
#### 1. 下載docker desktop https://hub.docker.com/editions/community/docker-ce-desktop-mac
    ``` bash
    $ cd ~/
    $ git clone https://github.com/laradock/laradock.git
    ```
#### 2. copy .env 檔
    ``` bash
	$ cd Laradock
	$ cp env-example .env
    ```

#### 3. 編輯 Laradock/.env 檔案。 
#### 4. Paths 部分：
    ``` bash
    $ mkdir Project
    ```
	APP_CODE_PATH_HOST=~/Project/
	MYSQL 部分：
	MYSQL_ROOT_PASSWORD=root (phpmyadmin 登入密碼）
	PhpMyadmin 設定：
	PMA_USER=root
	PMA_ROOT_PASSWORD=root123
	PHA_PORT=8082（PHPMyAdmin port ）
#### 5. Mac 編輯 /etc/host 檔案 
    ``` bash
    $ vi /etc/hosts
    ```
    加入
	127.0.0.1  test.test
	127.0.0.1 	test1.test (第二個專案）
### 6. 建立larval project
    方法1.
    查看各容器資訊 找到workspace 的 CONTAINER ID 
    ``` bash
	$ docker  ps   
	$ docker exec -it  CONTAINER ID  bash
    ```
	方法2.
    ``` bash
	$ docker-compose exec workspace bash
    ```
	進到容器後安裝laravel
    ``` bash
	$ composer create-project --prefer-dist laravel/laravel:^7.0 test .test
    ```
	test 是專案名稱
### 7. 設定Laravel 專案DB connect info
    ``` bash
    $ cd ~/Project/test.test 
    ```
	修改 .env檔
	DB_CONNECTION=mysql
	DB_HOST=mysql
	DB_PORT=3306
	DB_DATABASE=test
	DB_USERNAME=root
	DB_PASSWORD=root3456
### 8. 設定nginx
    ``` bash
	$ cd ~/Laradock/nginx/sites
	$ cp laravel.conf.example laravel.test.test.conf (複製範例檔案）
    ```
    root 是專案目錄 以自己專案資料夾名稱為主
#### 設定nginx
![nginx設定](/images/serversetting.png)
    error_log    
	access_log
	名稱皆以專案名稱為主
![errorlog](/images/errorlog.png)
以上設定完後 啟動容器
    ``` bash
    $ docker-compose up -d nginx mysql phpmyadmin
    ```
### 9. 設定Laravel admin 

首先 先建立本機的資料庫並設定好連線 專案的.env檔
![dbsetting](/images/dbsetting.png)
如果沒有切換到容器底下會噴錯 可能會有 沒有權限的錯誤所以一定要切到容器內再執行！!
先切換到容器裡頭
    ``` bash
    $ docker exec -it  workspace  /bin/bash
    root@xxxxxxxxxx:/var/www#  cd ~/ test/
    ```


在切換到 當前專案目錄底下
執行
    ``` bash
    $ composer require encore/laravel-admin:1.*
    ```
然後再執行來發布資源
    ``` bash
    $ php artisan vendor:publish --provider="Encore\Admin\AdminServiceProvider"
    ```
在該命令會生成配置文件config/admin.php，可以在裡面修改安裝的地址、資料庫連接、以及表名，建議都是用預設配置不修改。
執行完上頭指令接下來完成安裝
    ``` bash
    php artisan admin:install
    ```
### 10. 安裝完畢後 測試是否有安裝成功

http://localhost/admin/ 帳號 admin 密碼 admin

### 小結 
以上就是Laradock 建立專案的教學，希望能對你有所幫助，之後也會分享更多好玩的知識以及技術，希望大家能多多支持一下～～ 我是Yiuzzx 下次見～！

<iframe class="lc-margin-top-80 lc-margin-bottom-32 lc-mobile" data-v-b66e9a5a="" frameborder="0" src="https://button.like.co/in/embed/tarry5508/button?referrer=https://yliuzz.github.io/2021/12/02/laradock-%E5%A4%9A%E5%B0%88%E6%A1%88%E5%BB%BA%E7%BD%AE-x-laravel-admin-%E5%BB%BA%E7%BD%AE/&amp;" style="font-size: medium;" ></iframe>

