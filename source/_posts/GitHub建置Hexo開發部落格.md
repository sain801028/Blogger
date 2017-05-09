---
title: GitHub建置Hexo開發部落格
date: 2017-04-20 12:21:19
tags: Hexo
categories: Blogger
---



## 環境 (前置)

### 安裝git
[git 官網下載最新版(winsows)](https://git-scm.com/)

### 安裝node.js
[nods.js 官網下載最新版(windows)](https://nodejs.org/en/)


## Github 與 SSH 連結 (前置)

### 建立一個專案
專案名稱為 username.github.io

### GitConfig
```bash
$ git config --global user.name "Your Username"
$ git config --global user.email "Your Email"
```

### 產生SSH

檢查是否產生ssh過
```bash
$ cd ~/.SSH
```

產生SSH
```
$ ssh-keygen -t rsa -C "Your Email"
```

此時應該會跳出訊息
```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/SherD/.ssh/id_rsa): Enter跳過
Enter passphrase (empty for no passphrase): Enter跳過
Enter same passphrase again: Enter跳過
```

### 複製 rsa金鑰 github
```
cat ~/.ssh/id_rsa.pub | pbcopy
```

在github的帳號設定(Personal settings)裡面有個 SSH and GPG keys
在SSH keys 點選 New SSH Keys  

```
Title 金鑰名
Key 貼上剛才的rsa金鑰
```
點選add SSH key

### 確認SSH key 是否正常
```bash
$ssh -T git@github.com
```

第一次會出現選項 請打yes
```
The authenticity of host 'github.com ()' can't be established.
RSA key fingerprint is xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx.
Are you sure you want to continue connecting (yes/no)?  (yes並enter)
```

出現下面訊息代表成功，此時就可使用ssh 存取github
```
Hi Sher! You've successfully authenticated, but GitHub does not provide shell access.
```


## Hexo建置

### 開始建置部落格
創建一個blog資料夾，並在資料夾中執行 git bash

### 安裝hexo
```bash
$ npm install hexo-cli -g
```

### Hexo 初始化 會建立所有相關套件
```bash
$ hexo i
```

### 主題安裝
```bash
$ git clone git@github.com:yscoder/hexo-theme-indigo.git themes/indigo
```

### config.yml 設定檔編輯
編輯blog資料夾根目錄的config.yml 將裡面的theme 改為indigo

### 添加依賴套件
```bash
$ npm install hexo-renderer-less --save
$ npm install hexo-generator-feed --save
$ npm install hexo-generator-json-content --save
```

### 本地端測試 (可在 localhost:4000 預覽網頁)
``` bash
$ hexo s
```

### 生成靜態資料
``` bash
$ hexo g
```

### 再次修改 config.yml 設定檔 最下面的deploy 改為
```
deploy:
  type: git
  repository: git@github.com:D-Sher/d-sher.github.io.git  (your sshgit)
  branch: master
  message: update
```

###安裝部屬套件
```bash
$ npm install hexo-deployer-git --save
```

### 部屬到網站

``` bash
$ hexo d
```

## 使用Hexo 寫你的第一篇文章

### 建立文章
```bash
$ hexo n (文章名稱)
```


此時可在 ``source/_posts/(文章名稱).md`` 編輯 markdown 檔

### 加入圖片套件
可以增加 一個圖片位置套件
```bash
$ npm install hexo-filter-pathfix --save
```

此套件在嵌於網站的資源(圖片 資料)擁有正確的路徑

例如實際的位置  ``source/_posts/用github建置Hexo開發部落格/1.png``

這邊記得要修改跟目錄的 config.yml 裡面的
```
post_asset_folder: true
```

我們即可利用嵌入碼
```
![範例圖片](1.png)
```

最後顯示
![範例圖片](1.png)

## 更新

### 新增或編輯文件後 生成靜態檔案後部屬 網站即便更新
``` bash
$ hexo g
$ hexo d
```

## 備份
### 備份Hexo的檔案  

在跟目錄的 config.yml 底下新增一個backup
```
backup:
    type: git
    repository:
       github: git@github.com:D-Sher/d-sher.github.io.git,Hexo (最後面斜線為分支名稱)
```

### 安裝備份套件
``` bash
$ npm install hexo-git-backup --save
```

### 備份到分支
``` bash
$ hexo b
```
