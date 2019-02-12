---
title: vue中使用protobuf踩坑记
date: 2018-06-27 10:16:22
tags: vue
categories: FE
keywords: vue protobuf
---
> 因公司需求需使用protobuf，然后就有了这个踩坑记录

# 什么是protobuf？
官方解释为：
> Protocol buffers are a flexible, efficient, automated mechanism for serializing structured data – think XML, but smaller, faster, and simpler. You define how you want your data to be structured once, then you can use special generated source code to easily write and read your structured data to and from a variety of data streams and using a variety of languages. You can even update your data structure without breaking deployed programs that are compiled against the "old" format.

翻译是(机翻---我英语不好)
> 协议缓冲区是用于序列化结构化数据的灵活，高效的自动化机制 - 思考XML，但更小，更快，更简单。您可以定义一次数据的结构，然后您可以使用特殊的源代码轻松地将结构化数据写入各种数据流并使用各种语言读取和读取数据。您甚至可以更新您的数据结构，而不会中断根据“旧”格式编译的已部署程序。

特点：
- 更简单
- 是3到10倍小
- 速度要快20到100倍
- 不太模糊
- 生成更易于以编程方式使用的数据访问类

# 代码
> 在github上写了个demo
> [demo地址](https://github.com/rsl140/work_notes_demo/tree/master/proto_demo) 有需要的可以下载下来跑一下就理解了。PS:如果觉得有用 请给我个小星星 (笔芯~)

# 使用

其实最开始我尝试使用一个第三方JS  [protobuf.js](https://github.com/dcodeIO/protobuf.js)
`protobuf.load`的时候浏览器报了个错[illegal token '<' (/demo.proto, line 1)](https://github.com/dcodeIO/protobuf.js/issues/1023)查找了下官网issue,大意应该是proto文件多了个字符，但是我查看过proto文件并没有发现有多的'<'，怎么办呢，最后放弃使用第三方。用官方提供的方法。

## 下载protobuf编译器
[下载地址](https://repo1.maven.org/maven2/com/google/protobuf/protoc/) (我下载的是3.40版)
[github也提供了zip包，可自行下载](https://github.com/google/protobuf/releases) (目前最新版本是v3.6.0)
用来编译proto为JS文件方便调用

## 配置环境变量
由于公司用的是win10 只需要将下载的文件地址添加到path即可
Mac与window命令唯一的区别就是需要将protoc改成protoc.exe 前提是需要添加环境变量
[配置环境可参考这篇文章](https://www.jianshu.com/p/e63082e9188d)

## 编写proto文件

为了确保前后一致，下面是后台写给我的一个测试proto，我司后台是java
``` bash
syntax = "proto2";//protobuf版本
option java_package = "com.test.protobuf";
option java_outer_classname = "PersonMessage";
message Person {
  required int32 id = 1;
  optional string name = 2;
  optional string email = 3;
  repeated string list = 4;
  extensions 100 to 1000;//允许扩展的ID
}

message PersonTree {
  optional string id = 1;
  optional string title = 2;
  repeated PersonTree childs = 3;
}

extend Person {
  optional int32 count = 101;
  optional int32 likes_cnt= 102;
}

message PersonEx {
  optional int32 id = 1;
  extend Person {
    optional int32 px = 103;
    optional int32 py= 104;
  }
  optional Person p = 2;
}
```
## 使用vue-cli构建一个工程目录
``` bash
npm install -g vue-cli
vue init webpack my-project
cd my-project
npm install
npm run dev
```
安装插件：`npm install axios element-ui google-protobuf --save`

## 编译proto为JS
进入`awesome.proto`的存放路径 使用如下命令
`protoc.exe --js_out=import_style=commonjs,binary:. awesome.proto`
- 会生成一个`awesome_pb.js`文件
- 点击查看`awesome_pb.js`其实可以看到里面是生成好的方法。只需要在页面中引入JS调用即可

之后我们将这个文件引入页面，当然你也可以考虑全局引用




## 测试

### 本地测试
编写一个测试页面,创建一个测试按钮
我是在测试页面`import messages from './awesome_pb.js'`
方法为：
``` js
methods: {
    protobufferTest () {
      var message = new messages.Person() // 调用Person对象  实例化
      // 赋值
      message.setId(23)
      message.setName('asd')
      // 序列化
      var bytes = message.serializeBinary()

      console.log(bytes) // Uint8Array(7) [8, 23, 18, 3, 97, 115, 100]

      // 反序列化
      var message2 = messages.Person.deserializeBinary(bytes)

      console.log(message2) // proto.PersonTree {wrappers_: null, messageId_: undefined, arrayIndexOffset_: -1, array: Array(3), pivot_: 1.7976931348623157e+308, …}

    }
  }
```
到此，本地测试完成，没什么毛病了。

### 前后端联调测试
前方有坑

前后传输是使用的FormData，然后悲剧的事情来了。后台解析不了。查看了下数据 `[8, 23, 18, 3, 97, 115, 100]`确实是传过去了。
<del> 后来排查出原因是应该是解析成了字符串，然后数值变了。所以解析不出来。
后来使用fromCharCode()方法编辑成字符串形式传输给后台。在使用charCodeAt()取值。
``` js
此方法已弃用
protobufferTest () {
      var message = new messages.Person() // 调用Person对象  实例化
      // 赋值
      message.setId(23)
      message.setName('asd')
      // 序列化
      var bytes = message.serializeBinary()

      console.log(bytes) // Uint8Array(7) [8, 23, 18, 3, 97, 115, 100]

      var tests = ''
      for (let index = 0; index < bytes.length; index++) {
        tests += String.fromCharCode(bytes[index])
      }
      console.log(tests) // asd

      // 存入FormData
      let uploadDatas = new FormData()
      uploadDatas.append('protobuf', tests)

      // 使用axios传输给后台
      this.axios.post('/test', uploadDatas)
        .then(function (response) {
          // 将传回的字符串转为数组
          console.log(response.data.split('')) // ["↵", "", "3", "2", "", "", "a", "s", "d", "f"]
          let str = response.data.split('')
          let toChar = []
          for (let index = 0; index < str.length; index++) {
            toChar.push(str[index].charCodeAt())
          }
          console.log(toChar) // [10, 2, 51, 50, 18, 4, 97, 115, 100, 102]

          // 后台传回来的是PersonTree里面的值所以调用PersonTree来反序列化
          var message2 = messages.PersonTree.deserializeBinary(toChar)

          console.log(message2) // proto.PersonTree {wrappers_: null, messageId_: undefined, arrayIndexOffset_: -1, array: Array(3), pivot_: 1.7976931348623157e+308, …}

          // 获取PersonTree的id值
          console.log(message2.getId()) // 32
        })
        .catch(function (error) {
          console.log(error)
        })

    }
```
</del>

以上方法可能存在安全隐患。
向后端传值
因为FormData支持两种方式传输string和blob所以将bytes存入blob中
前端获取数据
对axios的默认传输方式做个更改
axios.defaults.responseType = 'arraybuffer'
将以上的JS代码更改为以下内容
``` js
protobufferTest () {
      var message = new messages.Person()
      message.setId(23)
      message.setName('asd')
      var bytes = message.serializeBinary()

      console.log(bytes)
      let uploadDatas = new FormData()
      var blob = new Blob([bytes], {type: 'application/octet-stream'})

      uploadDatas.append('protobuf', blob)

      this.axios.post('/test', uploadDatas)
        .then(function (response) {
          console.log(response)

          var message2 = messages.PersonTree.deserializeBinary(response.data)
          console.log(message2.getId())
        })
        .catch(function (error) {
          console.log(error)
        })
      // console.log(bytes)
    }

```
至此前后联调完成

# 参考文档
[前端后台以及游戏中使用google-protobuf详解](https://www.jianshu.com/p/e63082e9188d)
[javascript前端如何使用google-protobuf](https://blog.csdn.net/arvin_kai/article/details/77532595)


