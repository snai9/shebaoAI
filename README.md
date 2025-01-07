# 修改aardio作者的范例，弄了个自己的AI机器人
## 为什么叫shebaoAI
- 弄的时候只是想学技术而已，所以拿社保作了测试
- 作为一个用户可以通过AI助手查询相关的社保知识
- 作为操作者也可以通过AI对不熟悉的业务进行查询
## 做了哪些修改
- UI全部自己用标准组件重写的，主要是精简功能
- 因为智谱AI有自己的知识库，所以增加了个知识库的开关
- 输入框拖入网址可作为临时知识库进行查询
- 拖入文件可直接上传到智谱的知识库
## 学习心得
- 富文本框的文件拖入
- 有些封装好的回调函数可能需要手动调用，因为要满足事件的触发条件，比如这里的
  ```aardio
  if (!mainForm.chkFix.checked) {
    			mainForm.chkFix.checked = true; // 如果 chkFix 不是 true,先切换为 true
    			mainForm.chkFix.oncommand();//手动调用 chkFix.oncommand 事件,满足选中状态的触发条件
  ```
  - aardio库的写法及调用方法，以后慢慢弄明白库的类和名字空间，有点晦涩
  - 智谱AI模型API的用法，以后还要进一步熟悉
  - string.replace的用法
    ```aardio
    string.replace("原始字符串", "查找字符串", "替换字符串", 替换次数)
    string.replace("原始字符串", "模式表达式", "替换字符串", 替换次数)
    ```
    个人理解，如果第三个参数是回调函数，那函数的参数就应该是模式匹配上以后的结果
  - 多线程的用法，库要单独导入，使用到的变量也要当参数传进去
  - 标准控件有很多封装好的回调函数，比如onok，就是直接回车替代点击确实按钮
  - 构建表单数据的固定格式，空行都不能错
  ```aardio
  // 生成 boundary 分隔符
        var boundary = "----WebKitFormBoundary" ++ ..string.random(16);

        // 构建表单数据 方法1
        var bodyy = "--" ++ boundary ++ '\r\n'; // 开始一个部分
        bodyy = bodyy ++ 'Content-Disposition: form-data; name="files"; filename="' ++ filename ++ '"\r\n'; // 描述在知识库中显示的名称,必须有后缀
        bodyy = bodyy ++ "Content-Type: application/octet-stream" ++ '\r\n\r\n'; // 描述文件类型,一定要是两个,还是是单引号
        bodyy = bodyy ++ fileContent ++ '\r\n'; // 文件内容,可以是字符串也可以是二进制读取的
        bodyy = bodyy ++ "--" ++ boundary ++ "--\r\n";  //结束整个请求体
        
        // 构建表单数据 方法2
		var body = string.concat(
    		"--", boundary, '\r\n', // 开始一个部分
    		'Content-Disposition: form-data; name="files"; filename="', filename, '"\r\n', // 描述在知识库中显示的名称,必须有后缀
    		"Content-Type: application/octet-stream\r\n\r\n", // 描述文件类型,一定要是两个,还是是单引号
    		fileContent, '\r\n', // 文件内容,可以是字符串也可以是二进制读取的
    		"--", boundary, "--\r\n"  // 结束整个请求体
		);
  ```
  - 说到构建请求体，不得不提到最终帮到我的超牛的工具Postman，以后用得上
  - aardio中，请求头要单独设置，不是作为参数传递给http.post()的，这和python不一样，好像python可以把url,请求头，请求体一起传的
  

  
