## 关于必须删除node_modules包在npm i才可以 正常运行的一种论证

### 三个文件夹介绍
1. main项目为平时开发的主项目，其中会依赖module1和module2  
```js
require('@zwhkk/module1')
```

2. module2  
module2@2.0.0中 global.module2 = 'variable module2, v2.0.0'
module2@2.0.1中 global.module2 = () =>  'function module2, v2.0.1'

3. module1  
```js
require('@zwhkk/module2')

console.log('module1中打印global.module2: ', global.module2)
console.log('module1中打印global.module2(): ', global.module2())
```
### 复现步骤
```bash
# 查看node_modules中依赖包关系
npm ls

# 进入main项目,
cd main

# 几个月安装了module2@2.0.0
npm i @zwhkk/module2@2.0.0 -S

# 现在module2已经升级为2.0.1, 且module1依赖形式为 "@zwhkk/module2": "^2.0.0"
npm i @zwhkk/module1 -S

# 安装结果为下面，
# @zwhkk/module1@2.0.0
# @zwhkk/module2@2.0.0

# 执行文件, 此时global.module2是变量不是函数，所以报错
node index.js

# 删除node_modules和package-lock.json，然后npm i，此时global.module2是函数，正常运行
npm i
# @zwhkk/module1@2.0.0
# @zwhkk/module2@2.0.1

# 执行文件, 此时global.module2是函数，正常运行
node index.js
```