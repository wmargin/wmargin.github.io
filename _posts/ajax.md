# 1.Ajax解析纯文本数据

## 请求步骤

1. 获取XHR对象

   ```javascript
   var request = new XMLHttpRequest()
   ```

2. 配置Ajax请求参数：<u>请求类型</u>、<u>请求的URL资源</u>、<u>请求方式，默认为true，即异步</u>

   ```javascript
   request.open('GET',this.href+'?='+(new Date()),true);
   ```

3. 将Ajax请求发送到服务器，GET请求不需要参数，可为空或者传入null

   ```javascript
   request.send(null)
   ```

4. Ajax对象的事件属性 `onreadystatechange` ,不断监听服务器端的响应

   ```javascript
   request.onreadystatechange = function(){
       //5.当请求状态为4，并且响应码是200时，表示服务器响应成功，并返回了用户数据
       if(request.readyState == 4 && request.status == 200){
           //6.根据数据类型，来更新当前页面中的DOM节点的信息
           document.getElementsByTagName("h3")[0].innerHTML = request.responseText;
       }
   }
   ```

- `request.abort()`在接受请求之前将请求中断

## 请求头

`var myHeader = request.getResponseHeader("请求头名")`
`var allHeaders = request.getAllResponseHeaders()`

# 2.readyState

- 0	未初始化
- 1     启动
- 2     发送
- 3      接受 --断接受服务端响应数据
- 4      完成  --的数据已经返回完成

**整个请求过程中会不断触发  `onreadystatechange`**

# 3.AJAX访问html文件

1. ```javascript
   var content = document.getElementById("content")
   
   content.innerHTML = request.responseText;
   ```

   

# 4.AJAX解析XML格式数据

1. 请求到XML文件以后，解析当前XML文件中的每个节点的数据，保存到对应变量中，XML支持所有的DOM操作

   ```javascript
   var name = result.getElementsByTagName("name")[0].childNodes[0].nodevalue;
   //可以用firstChild.nodevalue 进行代替
   ```

   解析完成以后，其实我们得到的就是字符串

2. 将解析后获取的XML节点数据，插入到当前页面中应的DOM节点中

   ```javascript
   document.getElementById("name").innerHTML = name;
   ```

   

# 5.AJAX访问Json数据



- 解析JSON数据

  ```javascript
  JSON.parse(request.responseText)
  ```

  

# 5.jQuery 使用 $.ajax()

```javascript
$.ajax ({
    type:"GET",
    url:"demo.json",
    data:null, //{}
    dataType:"json",
    success:function (data){
        
    }
})
```

## 1.load()方法

- jQuery对象的方法

- `load()方法三个参数，第一个参数是 请求的url    第二个参数是  要传递的数据，多个数据可以用对象传递   第三个参数是回调函数`

- 获取html文件的内容，可以使用jQuery选择器，按需获取

  `.load("lesson.html  li:first")`

  `.load("lesson.html  li:eq(2)")`

  `.load("lesson.html  li:odd")`

- 获取xml中的数据

  `.load("who.xml  work")`  <u>显示指定标签名的数据</u>

- 获取json数据

- 通过回调函数解析后再使用

  将JSON字符串转化为js对象

  ```javascript
  .load("demo.json",function(data) {
      var jsonObj = JSON.parse(data)
      $(this).empty() //清空当前显示的json字符串内容
  })
  ```

  

## 2.$.get()函数

## 3.$.getJSON()函数

**专门用来处理JSON数据**

- 不用使用 `JSON.parse()`解析得到的数据，可以直接使用

## 4.$.post()函数

```javascript
$("#name").change(function(event){
    $.post(
    "check.php",  //请求的url地址，处理脚本
     {"name":$(this).val()},  //发送给处理脚本的数据，js对象格式
     function (data){
         $("#name").next().empty();  //清空前一次显示结果
         $("#name").after(data)  //输出响应结果
     }
    )
})
```

# 6.跨域通信

- **CORS**
- **JSONP**

