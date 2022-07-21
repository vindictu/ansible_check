# Modules
## blockinfile
### parameters
- path: 要修改文件的路径。
	- string
- marker: 标记模版，{mark}会替换为marker_begin和marker_end中的值,如果用户不使用{mark}多次执行playbook会导致重复插入。
	- string
	- default: "# {mark} ANSIBLE MANAGED BLOCK"
- marker_begin: block开始的标识模版。
	- string
	- default: BEGIN
- marker_end: block结束的标识模版。
	- string
	- default: END
- block: 文件中要插入的内容。如果插入内容为空则认为是删除 state: absent。
	- string
	- default: ""
- insertafter: 如果没有找到开始/结束marker行,则将在指定正则表达式的最后一个匹配之后插入该块。特殊值EOF用于在文件末尾插入块,如果指定的正则表达式没有匹配，该块将被插入到文件的末尾。
	- string
	- default: EOF
- insertbefore: 如果没有找到开始结束的marker行,则将在指定正则表达式的最后一个匹配之前插入模块。特殊值BOF用于在文件开头插入块,如果指定正则表达式没有匹配，该块将被插入到文件的末尾。
	- string
	- default: BOF
- create: 如果文件不存在则创建一个新文件。
	- boolean
	- default: no
- state：指定block是添加还是删除。
	- string
	- default: present


## set_fact
### parameters
- cacheable: 创建了变量的 2 个副本，一个具有高优先级的普通"set_fact"主机变量和一个较低的"ansible_fact"
	- boolean
	- default: no
- key/value: key: value定义变量，变量值可以是布尔、字符串、列表、字典 
	- string

## wait_for
### parameters
- host: 要等待的主机名或ip地址。
- port：要轮寻的端口号。与path互斥
- timeout：等待的最大秒数。
- delay：开始轮询之前等待的秒数。
- state：检查端口started时将确保端口打开，stopped检查端口是否关闭，drained检查活动连接。
- path：在继续之前必须存在的文件系统上的文件的路径。与port互斥
- search_regex: 可用于匹配文件或套接字连接中的字符串。

1. <font color="#FF0000"> 一般使用: <br> delegate_to: localhost <br> connection: local  <br>指定执行主机等待检测目的节点的端口或文件 </font>

2. <font color="#FF0000"> 可以通过vars覆盖内置变量完成连接参数(port、passwortd等等)的修改 <br> vars: <br> &emsp; ansible_port: 50001</font>

## assert
### parameters
- fail_msg: 客户编辑的错误输出。
	- string
- success_msg: 客户编辑的正确输出。
	- string
- that：条件判断语句，和when相似。
	- list

## file
### parameters
- path: 管理文件、目录的路径。
	- string
- state：[absent, directory, file, touch, hard, link]
	- string
	- default: file
- owner: 所属用户。
	- string
- group：所属组。
	- string
- mode: 文件、目录的权限,u+rwx或u=rw,g=r,o=r。
- recurse: 与目录模式一起使用，递归修改目录下所有。
	- boolean
	- default: no
