---
{"dg-publish":true,"permalink":"/1.工具/npm/","created":"2024-04-28T21:12:04.504+08:00"}
---

npm（Node Package Manger，即node包管理器），是node.js默认的，用JavaScript编写的软件包管理系统。

# npm install报错处理
参考资料：
【1】[npm install时,报错 install: `node install.js`安装失败_芸灵fly的博客-CSDN博客](https://blog.csdn.net/weixin_38187317/article/details/84782923?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf) 

npm install时,报错：`install: node install.js安装失败`

解决方案：
1.加参数：`npm install --ignore-scripts`
`--ignore-scripts`表示npm将不会运行在package.json中指定的scripts脚本