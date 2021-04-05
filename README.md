# 跨域
## CORS
```JavaScript
//在响应头中加入 
response.setHeader('Access-Control-Allow-Origin','http://127.0.0.1:9990')
```
## JSONP
```JavaScript
//在响应头中加入 
    response.statusCode = 200
    response.setHeader('Content-Type', 'text/javascript;charset=utf-8')
    const string = `window['{{xxx}}']({{data}})`
    const data = fs.readFileSync('./public/friends.json').toString()
    const string2 = string.replace("{{data}}",data).replace("{{xxx}}",query.callback)
    response.write(string2)
    response.end()
   
   
   //执行的js 
   function jsonp(url) {
    return new Promise((resolve, reject) => {
        const random = "frankJSONPCallbackName" + Math.random();
        window[random] = data => {
            resolve(data);
        };
        const script = document.createElement("script")
        script.src = `${url}?callback=${random}`
        script.onload = () => {
            script.remove()
        }
        script.onerror = () => {
            reject();
        }
        document.body.appendChild(script)
    })
}

jsonp('http://localhost:8888/friends.js').then((data)=>{
    console.log(data);
})
```
我们在跨域的时候,当前浏览器不支持CORS,或是某种原因不支持CORS.那么就可以使用jsonp方式,就是请求一个js文件,这个js文件会执行一个回调,回调里面就有我们的数据.
优点什么就是 兼容IE ,跨域
缺点, 因为是Script 标签 我们只是知道成功失败, 不想Ajax 可以知道状态码,响应头,Script 只能发git的请求 不支持psot请求
