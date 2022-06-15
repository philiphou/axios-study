## Axios 学习
* axios的理解与使用
    - 前端最流行的 ajax 请求库
* json-server 搭建 REST API
    - json-server 是用来快速搭建模拟的 REST API 的工具包
        1. 下载： npm install -g json-server
        2. 目标根目录下创建数据库 json 文件： db.json
            {
                "posts": [
                    { "id": 1, "title": "json-server", "author": "typicode" }
                ],
                "comments": [
                    { "id": 1, "body": "some comment", "postId": 1 }
                ],
                "profile": { "name": "typicode" }
            }