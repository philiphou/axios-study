## Axios 学习
* axios的理解与使用
    - 前端最流行的 ajax 请求库
* json-server 搭建 REST API
    - json-server 是用来快速搭建模拟的 REST API 的工具包
        1. 下载： npm install -g json-server
        2. 目标根目录下创建数据库 json 文件： db.json
                    {
                "posts": [
                    { "id": 1, "title": "json-server", "author": "typicode" },
                    { "id": 2, "title": "他来了他来了", "author": "小编" }
                ],
                "comments": [
                    { "id": 1, "body": "some comment", "postId": 1 }
                ],
                "profile": { "name": "typicode" }
            }
        3. 启动 json server: 
                json-server --watch db.json
* 安装 axios
    - 项目中一般会使用 npm install axios 或者 yarn add axios
    - 也可以 CDN 通过 script 引入，学习阶段可以使用此方法
* request config:
    {
        url:"/user",
        method:"GET",
        baseURL:"http://some-domain/api/",
        transformRequest：[function(data){...return data}],
        params:{
            id:5530,
            age:518
        }
    }