# BOM

- 什么是BOM
  - Browser Object Model
  - 专门操作浏览器窗口的API——没有标准，有兼容性问题
- 浏览器对象模型
  - window——代表整个窗口
  - history——封装当前窗口打开后，成功访问过的历史url记录
  - navigator——封装浏览器配置信息
  - document——封装当前正在加载的网页内容
  - location——封装了当前窗口正在打开的url地址
  - screen——封装了屏幕的信息
  - event——定义了网页的事件机制
- 计时器
  - 周期定时器
    - 让城乡按指定时间间隔反复自动执行一项任务
    - setInterval（exp，time）
    - clearInterval（定时器名字）
  - 一次定时器
    - 让程序延迟一段时间执行