**Typora常见使用防范**

- 标题

  ```
  #       一级标题    
  ##      二级标题    
  ###     三级标题    
  ####    四级标题    
  #####   五级标题    
  ######  六级标题
  ```

- 引用

  ```
  > 引用内容1
  >> 引用内容2
  >>> 引用内容3
  ```

  - 效果

  > 引用1
  >
  > > 引用2
  > >
  > > > 引用3

- 单行代码

  `string s = "zhaokun"`

- 代码块

  ```java
  string s = "zhaokun"
  string s = "zhaokun"
  ```

- 无序表
  1. 有序表
  2. 有序表

- 插入图标

  ![名称]( 插入本地图片也可以插入网络图片)

  ![本地图片](../../img/Steve.jpg)

![网络图片](https://github.com/zhaokun00/note/raw/master/img/Steve.jpg)

- 插入表格

  |      |      |      |
  | ---- | ---- | ---- |
  |      |      |      |
  |      |      |      |
  |      |      |      |

- 插入超链接

  [百度][https://www.baidu.com/]

- 横向流程图

  ```mermaid
  graph LR
  
  A[方形] -->B(圆角)
  
      B --> C{条件a}
  
      C -->|a=1| D[结果1]
  
      C -->|a=2| E[结果2]
  
      F[横向流程图]
  ```

- 纵向流程图

  ```mermaid
  graph TD
  
  A[方形] -->B(圆角)
  
      B --> C{条件a}
  
      C -->|a=1| D[结果1]
  
      C -->|a=2| E[结果2]
  
      F[竖向流程图]
  ```

- 标准流程图

  ```flow
  st=>start: 开始框
  
  op=>operation: 处理框
  
  cond=>condition: 判断框(是或否?)
  
  sub1=>subroutine: 子流程
  
  io=>inputoutput: 输入输出框
  
  e=>end: 结束框
  
  st->op->cond
  
  cond(yes)->io->e
  
  cond(no)->sub1(right)->op
  ```

  

- 时序图

  ```sequence
  对象A->对象B: 对象B你好吗?（请求）
  
  Note right of 对象B: 对象B的描述
  
  Note left of 对象A: 对象A的描述(提示)
  
  对象B-->对象A: 我很好(响应)
  
  对象A->对象B: 你真的好吗？
  ```

  

```sequence
Title: 标题：复杂使用

对象A->对象B: 对象B你好吗?（请求）

Note right of 对象B: 对象B的描述

Note left of 对象A: 对象A的描述(提示)

对象B-->对象A: 我很好(响应)

对象B->小三: 你好吗

小三-->>对象A: 对象B找我了

对象A->对象B: 你真的好吗？

Note over 小三,对象B: 我们是朋友

participant C

Note right of C: 没人陪我玩
```

