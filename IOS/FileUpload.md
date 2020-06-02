---
title: FileUpload
date: 2019-05-1 22:31:12
tags: IOS
---

## 图片文件上传

采用`loopback`的 [LoopBack storage component](https://github.com/strongloop/loopback-component-storage)组件，可以实现上传文件或下载文件到云端网盘或者本地（服务器）的文件系统上。目前支持的云盘有：

- Amazon
- Azure
- Google Cloud
- Openstack
- Rackspace

国内的网盘，大多并不开放API，上面的网盘大多不能访问，或者就是速度不是很理想，所以采用了本地文件系统存储的方式。

### 关键概念

- **容器**： *容器* 将文件分组，类似于目录或者是文件夹。每个*容器* 定义了对象的命名空间并且使用唯一的名字来区分，通常是在用户账户内。

  注意：*容器*内不能有子*容器。*

- **文件**： 储存数据，例如文档或者是图像。每个文件总是在且仅仅在一个容器内。在容器内部，每个文件拥有唯一的名字。在不同容器里的文件可以有相同的名字。（这也就意味着，如果之前容器内已经有了同名文件，后上传的文件会覆盖之前的文件）

### 安装使用

安装模块：

```
$ npm install loopback-component-storage
```

使用模块：

```
$ lb datasource
[?] Enter the data-source name: picture
[?] Select the connector for myfile: other
[?] Enter the connector name without the loopback-connector- prefix: loopback-component-storage
[?] Install storage (Y/n)
```

在数据库配置文件`datasource.json`文件中，修改配置信息：

```json
"picture": {
    "name": "picture",
    "connector": "loopback-component-storage",
    "provider": "filesystem",
    "root": "./server/storage"
  }
```

之后创建一个`model`，可以命名为`container`,将其连接到我们创建的`picture`数据源上。

```
$ slc loopback:model
? Enter the model name: Container
? Select the data-source to attach Container to: picture (loopback-component-storage)
? Select model's base class Model
? Expose Container via the REST API? Yes
? Custom plural form (used to build REST URL):
? Common model or server only? common
```

之后会自动生成一个`container.json`文件

```json
{
  "name": "Container",
  "plural": "containers",
  "base": "Model",
  "idInjection": true,
  "options": {
    "validateUpsert": true
  },
  "properties": {},
  "validations": [],
  "relations": {},
  "acls": [],
  "methods": {}
}
```

#### 新建记录Model

为了对文件上传的信息进行记录，还需要新建一个`model`,这个`model`的数据源为持久性存储，如数据库。必要的信息有`name`、`url`和`type`，下面是我配置好的`Storage-file.json`文件：

```json
{
  "name": "StorageFile",
  "plural": "StorageFile",
  "base": "PersistedModel",
  "idInjection": true,
  "options": {
    "validateUpsert": true
  },
  "properties": {
    "id": {
      "type": "number"
    },
    "type": {
      "type": "string",
      "default": ""
    },
    "name": {
      "type": "string",
      "default": ""
    },
    "url": {
      "type": "string",
      "default": ""
    }
  },
  "validations": [],
  "relations": {},
  "acls": [],
  "methods": {}
}
```

之后在`Storage-file.js`中将`StorageFile mode`l和` Container model`联系在一起,同时添加远程操作。

```js
'use strict';

var CONTAINER_URL = '/api/containers/';

module.exports = function (StorageFile) {
    StorageFile.upload = function (ctx, options, cb) {
        console.info(ctx.req);
        options = options || {};
        // Firstly, you must create folder  /server/storage/common
        ctx.req.params.container = 'common';
        /**
         * ctx.req    express request object
         * ctx.result express response object
         */
        StorageFile.app.models.Container.upload(ctx.req, ctx.result,
            options, function (err, fileObj) {
                if (err) {
                    return cb(null, {
                        code: 200,
                        message: 'fail',
                        error: err.message
                    });
                } else {
                    // The 'file'below should be the same as field name in the form
                    var fileInfoArr = fileObj.files.file;
                    var objs = [];

                    console.info(fileObj.files);
                    fileInfoArr.forEach(function (item) {
                        
                        objs.push({
                            name: item.name,
                            type: item.type,
                            url: CONTAINER_URL + item.container +
                                '/download/' + item.name,
                        });
                    });
                    console.log(objs);
                    StorageFile.create(objs, function (err, instances) {
                        if (err) {
                            return cb(null, {
                                code: 200,
                                message: 'fail',
                                error: err.message
                            });
                        } else {
                            var res = {
                                code: 200,
                                message: 'success',
                                data: instances
                            }
                            cb(null, res);
                        }
                    });
                }
            });
    };

    StorageFile.remoteMethod(
        'upload', {
            http: { verb: 'post' },
            description: 'Upload a file or more files',
            accepts: [
                { arg: 'ctx', type: 'object', http: { source: 'context' } },
                { arg: 'options', type: 'object', http: { source: 'query' } },
            ],
            returns: {
                arg: 'fileObject', type: 'object', root: true,
            }

        }
    );
};

```

 上传文件时需要为表单类型，表单的field名为file ，数据类型为`multipart/form-data`，`ios`开发实现表单上传<https://www.jianshu.com/p/a0e3c77d3164>

### 文件名添加时间戳

在`/server`新建文件`datasources.local.js`,添加限制文件代码

```js
'use strict';

var path = require('path');

module.exports = {
  picture: {
    
    // 限定上传文件大小为50M
    maxFileSize: 100 * 1024 * 1024,

    // 自定义文件名
    getFilename: function(fileInfo) {
      var fileName = fileInfo.name.replace(/\s+/g, '-').toLowerCase();
      var fileObj = path.parse(fileName);
      // 给文件名加上时间戳
      return fileObj.name + Date.now() + fileObj.ext;
    },
  },
};

```

### 测试

![1561816433516](C:\Users\wang\AppData\Roaming\Typora\typora-user-images\1561816433516.png)

返回数据

```json
{
"code":200,
"message":"success",
    "data":	[{
    "id":34,
    "type":"image/jpeg",
    "name":"4bafe474-a425-4ad4-89ec-6b5f3d846f201561816439332.jpg",
    "url":"/api/containers/common/download/4bafe474-a425-4ad4-89ec-6b5f3d846f201561816439332.jpg"
    }]	   	 		
}
```

多文件

返回数据

```json
{
    "code":200,
    "message":"success",
    "data":[{
       "id":35,
        "type":"image/jpeg",
        "name":"40d8652c-7839-47b2-9337-898db31f87301561816572238.jpg",
        "url":"/api/containers/common/download/40d8652c-7839-47b2-9337-898db31f87301561816572238.jpg"
    	},
        {
        "id":36,
        "type":"image/jpeg",
        "name":"993cae69-ea7c-4826-837a-098c5d2f633b1561816572247.jpg",
        "url":"/api/containers/common/download/993cae69-ea7c-4826-837a-098c5d2f633b1561816572247.jpg"}
        ]
}
```