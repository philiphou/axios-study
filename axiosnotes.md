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
* axios.defaults.baseURL="https://api.example.com";
* axios 拦截器 interceptors
    - 请求拦截器
       axios.interceptors.request.use(function(config){
        console.log("请求拦截器成功")；
        return config       },function(error){
            console.log("请求拦截器失败")
            return Promise.reject('error)
        })
    - 响应拦截器
     axios.interceptors.response.use(function(response){
        console.log("请求拦截器成功")；
        return response       },function(error){
            console.log("请求拦截器失败")
            return Promise.reject('error)})
* axios 取消请求 防抖功能： 
                <script>
                    // 获取按钮：
                        const btns = document.querySelectorAll('button')
                            // 2.声明一个全局变量： 
                        let cancel = null
                            //  设置 axios 默认配置： 
                        axios.defaults.method = "GET"; // 设置默认的额请求类型为GET
                        axios.defaults.baseURL = "http://localhost:3000"
                            //  给第一个按钮绑定事件 发送 GET 请求：
                        btns[0].onclick = function() {
                            // 4. 设置防抖功能，发送请求之前先检测上一个请求有没有执行完毕：如果 cancel 值不是 null 就说明上一次请求还没有完成；
                            if (cancel !== null) {
                                //  取消上一次请求
                                cancel()
                            }
                            axios({
                                url: "/comments",
                                //  1.添加配置对象的属性： 
                                cancelToken: new axios.CancelToken(function(c) {
                                    //  3. 将 c 的值赋值给 cancel
                                    cancel = c
                                })
                            }).then(res => {
                                console.log(res)
                                    //  请求完成后，cancel 重新赋值为 null
                                cancel = null
                            }).catch(e => {
                                console.log("出错了")
                            })
                        }
                        //  给第二个按钮绑定事件 取消请求：
                        btns[1].onclick = function() {
                                cancel()
                            }
                            //  为了显示取消效果，给 json-server 设置一个延时反应效果：  json-server --watch db.json -d 2000
                </script>
* axios 取消请求的自定义实现： 
                <script>
                    //  构造函数
                    function Axios(config) {
                        this.config = config;
                    }
                    //  原型的request() 方法
                    Axios.prototype.request = function(config) {
                            return dispatchRequest(config)
                        }
                        //  dispatchRequest 函数
                    function dispatchRequest(config) {
                        return xhrAdapter(config)
                    }
                    //  CancelToken 构造函数
                    function CancelToken(executor) {
                        //  声明一个变量
                        var resolvePromise
                            //  为实例对象添加属性：
                        this.promise = new Promise((resolve) => {
                            //  将 resolve 方法 赋值给 resolvePromise 变量
                            resolvePromise = resolve
                        })
                        //   调用 excutor 函数
                        executor(function() {
                            //  执行 resolvePromise 函数： 
                            resolvePromise();
                        })
                    }
                    // xhrAdapter()
                    function xhrAdapter(config) {
                        //  发送 ajax 请求 
                        return new Promise((resolve, reject) => {
                            //   1. 实例化对象
                            const xhr = new XMLHttpRequest()
                                // 2. 初始化： 
                            xhr.open(config.method, config.url)
                                // 3. 发送
                            xhr.send()
                                // 4. 处理结果：
                            xhr.onreadystatechange = function() {
                                if (xhr.readyState === 4) {
                                    if (xhr.status >= 200 && xhr.status < 300) {
                                        // 设置为成功状态
                                        resolve({
                                            status: xhr.status,
                                           statusText: xhr.statusText
                                        })
                                    } else {
                                        reject(new Error("请求失败"))
                                    }
                                }
                            }
                            //  关于取消请求的处理
                            if (config.cancelToken) {
                                //  对 cancelToken 实例对象身上的 promise对象指定成功的回调
                                config.cancelToken.promise.then(value => {
                                    xhr.abort()
                                })
                            }
                        })
                    }
                    //创建 axios 函数
                    const axios = Axios.prototype.request.bind(context)
                    console.dir(axios)
                    // 获取按钮，以上为模拟实现的代码
                    const btns = document.querySelectorAll('button')
                    let cancel = null
                        // 发送请求
                    btns[0].onclick = function() {
                        //  检查上一次的请求是否已经完成
                        if (cancel !== null) {
                            //     //  取消上一次的请求
                            cancel()
                        }
                        axios({
                            method: "GET",
                            url: "http://localhost:3000/comments",
                            // 1. 添加配置对象的属性：
                            cancelToken: new CancelToken(function(c) {
                                //  3. 将 c 的值赋值给 cancel
                                cancel = c
                            })
                        }).then(res => {
                            console.log(res)
                                //  请求完成后，cancel 重新赋值为 null
                            cancel = null
                        }).catch(e => {
                            console.log('请求出错了 哈哈哈')
                        })
                    }
                    //  给第二个按钮绑定事件 取消请求：
                    btns[1].onclick = function() {
                            cancel()
                        }
                        //  为了显示取消效果，给 json-server 设置一个延时反应效果：  json-server --watch db.json -d 2000
                </script>