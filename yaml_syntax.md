# 文件起始格式
```
# 以三个减号代表yaml文件的开始
---
...
```
# 字符串
```
# 一般来说字符串不需要用引号引起来，即使中间有空格也不需要引号，特殊情况需要使用引号
this is a lovely sentence
```
# 布尔类型
```
True or False
```
# 列表定义
```
# 第一种形式
hosts:
	- controller
	- compute
	- storage
	- haproxy
	- rabbitmq
	- mysql
	
# 第二种形式
hosts: ['controller', 'compute', 'storage', 'haproxy', 'rabbitmq', 'mysql']
```
# 字典定义
```
# 第一种形式
haproxy:
	internal_vip: '192.168.10.10'
	external_vip: '172.16.10.10'
	cluster_port: 8080

# 第二种形式
haproxy: {internal_vip: '192.168.10.10', external_vip: '172.16.10.10', cluster_port: '8080'}

访问变量中字典的key
{{ login.stdout }}
{{ login['stdout'] }}
更倾向于使用点号，除非key中包含点、空格、连字符等不允许出现在变量名的字符。
```

# 折行
- *|*: 保留换行符
- *>*：换行符转换空格
```
...
vars: 
	include_newlines: |
		exactly as you see
		will appear these three
		lines of poetry

	fold_newlines: >
		this is really a
		single line of text
		despite appearances
debug:
	msg: "{{ include_newlines }} {{ flod_newlines }}"
		
```
# 引用变量
```
foo: "{{ variable}}"
```
# 引号使用
```
双引号可以进行转译
单引号无转译
[] {} > | * & ! % # ` @ , 特殊字符开头需要使用引号
当时用debug模块是msg内容需要使用引号来获取空格，当字符串中有: 和 {{}}时需要引号进行识别否则会被判断为字典类型
```
