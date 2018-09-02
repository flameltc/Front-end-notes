# npm

### npm是什么？

- npm--->Node Package Manager，即node包管理器

- npm是用Node.js编写的，所以需要安装Node.js才能使用npm

- **npm升级--->运行`npm install npm@latest -g`**

- npm 查看信息的命令

  ```
  // 查看npm命令列表
  npm help

  // 查看各个命令的简单用法
  npm -l

  // 查看npm的版本
  npm -v

  // 查看node.js的版本
  node -v

  // 查看npm的配置
  npm config list -l

  // 搜索npm仓库，它后面可以跟字符串，也可以跟正则表达式
  npm search <搜索词>

  // 终端登录
  npm login
  ```

  ​

### 安装、更新、卸载npm软件包

- 本地

  ```
  // 本地安装--->指的是将一个模块下载到当前项目的node_modules子目录，然后只有在项目目录之中，才能调用这个模块
  npm install <packageName>
  这将node_modules在当前目录中创建目录（如果目录尚不存在），并将该软件包下载到该目录

  // 安装模块的特定版本，可以在模块名后面加上@和版本号
  npm install xxx@0.1.1
  npm install xxx@">=0.1.0 <0.2.0"

  // 指定所安装的模块属于哪一种性质的依赖关系，即出现在packages.json文件的dependencies或者devDependencies中
  // –save--->模块名将被添加到dependencies，可以简化为参数-S
  // –save-dev:--->模块名将被添加到devDependencies，可以简化为参数-D
  npm install <packageName> --save
  npm install <packageName> --save-dev
  --------------------------------
  npm install <packageName> -S
  npm install <packageName> -D

  // 本地更新
  npm update <packageName>
  该命令会先到远程仓库查询最新版本，然后查询本地版本。如果本地版本不存在，或者远程版本较新，就会安装

  // 本地卸载
  npm uninstall <packageName>
  这会从node_modules目录中删除软件包

  // 如果是通过--save安装的，要将它从依赖关系中删除package.json
  npm uninstall --save <packageName>

  // 如果是通过--save-dev安装的，要将它从依赖关系中删除package.json
  npm uninstall --save-dev <packageName>
  ```

- 全局

  ```
  // 全局安装--->指的是将一个模块安装到系统目录中，各个项目都可以调用。一般来说，全局安装只适用于工具模块
  npm install -g <packageName>

  // 更新全局下的某个包
  npm update -g <packageName>

  // 更新所有全局包
  npm update -g

  // 卸载全局包
  npm uninstall -g <packageName>
  ```

### package.json有什么用

- package.json--->管理本地安装的npm软件包，一个package.json文件包括以下内容：

  - 定义包的基本描述信息[name、version、description、author等]
  - 定义包的主程序入口模块标示[main]
  - 定义包的使用方式[scripts]


  - 定义包的依赖管理
    - **dependencies：这些软件包是应用程序在生产中所需的**
    - **devDependencies：这些软件包仅用于开发和测试**
  - 定义包的bug、license等其它信息

- 创建package.json

  ```
  // 启动一个命令行调查问卷，该问卷最后将在创建package.json该命令的目录中创建一个问卷
  npm init

  // 创建一个默认值的package.json
  npm init --yes

  // 一个package.json例子
  {
    "name": "demo", // 名称--->发布包后，可以通过该名称在npm上搜索到这个包,名称是唯一的
    "version": "1.0.0", // 版本号--->当包做了改动时需要修改版本号再发布
    "description": "", // 自述文件中的信息，或者一个空字符串 ""
    "main": "index.js", // 主程序入口文件
    "scripts": { 
      "test": "echo "Error: no test specified" && exit 1"  // npm script 命令行，自定义的npm脚本，npm的内置命令可以使用：npm 内容命令，比如npm test 和 npm start。其它命令要写成 npm run xxx 形式，键名代表npm脚本的命令，而值则代表实际执行的命令。
    },
    "keywords": [ // 描述项目的关键字
      "demo" 
    ],
    "author": "", // 作者
    "license": "ISC", // 认证信息
    "dependencies": { // 生产环境依赖，当安装该项目时，生产环境依赖也将同时安装
      "xxx": "^x.x.x" 
    },
    "devDependencies": {  // 开发环境依赖，这些依赖仅在开发时有效
      "yyy": "^y.y.y"
    }
  }
  ```

### node_modules的查找路径是怎样的

- node.js模块分类：

  - 内建模块（path、fs）
  - 文件模块
    - node_modules
    - 本地存在

- **node_modules查找路径--->从项目所在目录开始查找 node_modules ，如果在当前目录找不到，向上进入父级目录查找，并以此类推，直到查找到所需文件或找到根目录下为止**

- 例子

  ```
  console.log(module.paths)

  /home/dengpan0513/Desktop/test/demo/node_modules
  /home/dengpan0513/Desktop/test/node_modules
  /home/dengpan0513/Desktop/node_modules
  /home/dengpan0513/node_modules
  /home/node_modules
  /node_modules
  ```

  ​

### 