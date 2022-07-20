# 过滤器

## default过滤器
**通常在使用变量的时候应用，变量如果没有定义则使用default给出的值**
```
HOST: "{{ database_host | default('localhost') }}"
```

## 注册变量的过滤器
**register之后使用**
|名称|描述|
|---|---|
|failed|如果注册变量的值是任务failed则返回True|
|changed|如果注册变量的值是任务changed则返回True|
|success|如果注册变量的值是任务succeeded则返回True|
|skipped|如果注册变量的值是任务skipped则返回True|

## 文件路径的过滤器
**处理包含控制主机文件系统的路径的变量十分有效**
|名称|描述|
|---|---|
|basename|文件路径的文件名部分|
|dirname|文件路径中的目录|
|expanduser|文件路径中的~替换为用户目录|
|realpath|处理符号连接后的文件实际路径|


## 编写自己的过滤器

