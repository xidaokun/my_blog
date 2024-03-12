---
title: Backend Node
date: 2022-04-05 17:22:00
tags: Backend
---

### 1，简介
一个后台服务项目，可以存储用户的文件和数据，并将文件同步到第三方云盘。

### 2，功能
- 手机号注册/手机号登录
- OAuth授权
- 账户登录
- 密码修改

- API鉴权
- 数据管理
- 文件管理
- 个人云盘数据同步

- 数字货币fee(TODO)


### 3，部署
3.1 clone
{% blockquote %}
$git clone https://github.com/xidaokun/Cary.Project.Python.Backend.git
{% endblockquote %}

3.2 run
{% blockquote %}
$./run.sh start
{% endblockquote %}

### 4，APIS
##### 4.1 User manager
1.register user
{% blockquote %}
$curl -d '{"name":"cary", "password":"123456", "phone":"1234567890", "code":1111}' -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/user/register
{% endblockquote %}

{% blockquote %}
{ "_status": "OK" }
{% endblockquote %}

2.user login
{% blockquote %}
$curl -d '{"name":"cary", "password":"123456"}' -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/user/login
{% endblockquote %}

{% blockquote %}
{ "_status": "OK", "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9\.  
eyJpc3MiOiJiYWNrZW5kIiwiaWF0IjoxNjE5NzA1MTg2LCJleHAiOjE2MTk3MDU0ODYs  
ImF1ZCI6ImNsaWVudCIsInN1YiI6IjYwOGFiZDFjOWYzOTRmN2Q3ZjAxOWY5NSIsIm5hbWUiOiJj  
YXJ5Iiwic2NvcGVzIjpbIm9wZW4iXX0\.3wirR5I5UCUDmuCi96adm3SctPEOFPYQKf9gcAF9Ib8", "user_id": "608abd1c9f394f7d7f019f95", "user_name": "cary" }
{% endblockquote %}

3.verification code
{% blockquote %}
$curl -d '{"phone":"1234567890"}' -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/user/verification_code
{% endblockquote %}

4.change password
{% blockquote %}
$curl -d '{"password":"12345"}' -H "If-Match: token JWT_TOKEN" -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/user/change_pwd
{% endblockquote %}

##### 4.2 OAuth
{% blockquote %}
$curl http://127.0.0.1:5000/api/v1/user/oauth?code=your_access_code&type=github&redirect_uri=http://example.com&state=xyz
{% endblockquote %}

{% blockquote %}
{ "_status": "OK", "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9\.  
eyJpc3MiOiJiYWNrZW5kIiwiaWF0IjoxNjE5NzA1MTg2LCJleHAiOjE2MTk3MDU0ODYsImF1  
ZCI6ImNsaWVudCIsInN1YiI6IjYwOGFiZDFjOWYzOTRmN2Q3ZjAxOWY5NSIsIm5hbWUiOiJjY  
XJ5Iiwic2NvcGVzIjpbIm9wZW4iXX0\.3wirR5I5UCUDmuCi96adm3SctPEOFPYQKf9gcAF9Ib8", "user_id": "608abd1c9f394f7d7f019f95", "user_name": "cary" }
{% endblockquote %}

##### 4.3 File operation
1.upload file
{% blockquote %}
$curl -H "If-Match: token JWT_TOKEN" http://127.0.0.1:5000/api/v1/file/upload/test.txt
{% endblockquote %}

2.download file
{% blockquote %}
$curl -i -H "If-Match: token JWT_TOKEN" http://127.0.0.1:5000/api/v1/file/download?path=test.txt
{% endblockquote %}

3.list files
{% blockquote %}
$curl -i -H "If-Match: token JWT_TOKEN" http://127.0.0.1:5000/api/v1/file/list
{% endblockquote %}

4.get file information
{% blockquote %}
$curl -i -H "If-Match: token JWT_TOKEN" http://127.0.0.1:5000/api/v1/file/information
{% endblockquote %}

5.delete file
{% blockquote %}
curl -d '{"file":"test.txt"}' -H "If-Match: token JWT_TOKEN" -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/file/delete
{% endblockquote %}

##### 4.4 Database operation
1.create collection
{% blockquote %}
$curl -d '{"collection":"workers"}' -H "If-Match: token JWT_TOKEN" -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/db/create_col
{% endblockquote %}

2.insert one/many document
{% blockquote %}
$curl -d '{"collection":"workers","document":{"worker":"cary","title":"developer"},"options":{"bypass_document_validation":false}}' -H "If-Match: token JWT_TOKEN" -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/db/insert_one
{% endblockquote %}

3.update one/many document
{% blockquote %}
$curl -d '{"collection":"workers","filter":{"worker":"cary",},"update":{"$set":{"author":"cary","title":"manger"}},"options":{"upsert":true,"bypass_document_validation":false}}' -H "If-Match: token JWT_TOKEN" -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/db/update_one
{% endblockquote %}

4.count documents
{% blockquote %}
$curl -d '{"collection":"workers","filter":{"worker":"cary",},"options":{"skip":0,"limit":10,"maxTimeMS":1000000000}}' -H "If-Match: token JWT_TOKEN" -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/db/count_documents
{% endblockquote %}

5.delete one/many documents
{% blockquote %}
$curl -d '{"collection":"workers","filter":{"worker":"cary",}}' -H "If-Match: token JWT_TOKEN" -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/db/delete_one
{% endblockquote %}

6.delete collection
{% blockquote %}
$curl -d '{"collection":"workers"}' -H "If-Match: token JWT_TOKEN" -H 'Content-Type: application/json' http://127.0.0.1:5000/api/v1/db/delete_col
{% endblockquote %}


