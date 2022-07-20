# Play
### environment
```
字典格式，在任务执行时被转化为环境变量使用。只能与modules一起使用，其他任何类型的插件、ansible本身、配置都不受支持。
只在task执行时被赋值，不应该用它传递机密数据。
```
### gather_facts
```
布尔格式，控制play是否自动运行setup模块取搜集远程主机的数据。
```
### hosts
```
列表格式，由hosts、groups、pattern转换而成的主机列表。
```
### port
```
字典格式，在连接的时候替换默认连接端口
```
### run_once
```
布尔格式，跳出主机循环，强制task在第一个host中执行，并把返回结果应用给所有同一批次的主机。
```
### tags
```
应用与tasks或included tasks，允许用户在命令行中选择tasks子集执行。
```
### pre_tasks
```
tasks组成的列表格式，在roles部分之前执行
```
### roles
```
由role组成的列表格式，导入到play中执行。
```
### tasks
```
tasks组成的列表格式，在roles之后、post_tasks之前执行。
```
### post_tasks
```
tasks组成的列表格式，在tasks部分之后执行。
```
### timeout
```
tasks执行的时间限制，超出限制ansible会终止并导致task执行失败
```
### vars
```
字典/映射格式
```
### vars_files
```
由文件组成的列表格式，文件中包含了要引入play的vars
```
### serial
```
某一段时间内有几个远程主机在执行task
```
### max_fail_percentage
```
百分比，最大失败主机比例达到多少时ansible就让整个play失败，0代表任何主机出现task执行失败都放弃执行。
```

# Role
### delegate_to
```
指定执行task的主机而非target (inventory_hostname)，来自委派主机的连接变量也会作用与该任务。
```
### environment
```
字典格式，在任务执行时被转化为环境变量使用。只能与modules一起使用，其他任何类型的插件、ansible本身、配置都不受支持。
只在task执行时被赋值，不应该用它传递机密数据。
```
### ignore_errors
```
布尔格式，允许你忽略task中的错误并继续运行play，无法忽略连接错误。
```
### ignore_unreachable
```
布尔格式，允许你忽略连接错误继续执行play，但无法忽略task执行中的错误。
```
### run_once
```
布尔格式，跳出主机循环，强制task在第一个host中执行，并把返回结果应用给所有同一批次的主机。
```
### tags
```
应用与tasks或included tasks，允许用户在命令行中选择tasks子集执行。
```
### timeout
```
tasks执行的时间限制，超出限制ansible会终止并导致task执行失败
```
### vars
```
字典/映射格式
```
### when
```
条件表达式，判断是否执行任务迭代。
```
# Block
### always
```
由task组成的列表，无论block中是否有错误都执行。
```
### rescue
```
由task组成的列表，block中task存在执行错误时执行rescue指定的task
```
### notify
```
handlers组成的列表，当task返回changed=True时执行。
```
<font color="#dd0000">其他关键字与role一致</font>
- delegate_to
- environment
- ignore_errors
- ignore_unreachable
- run_once
- tags
- timeout
- vars
- when
# Task
### async
```
执行异步任务，值是以秒为单位的最大运行时间
```
### poll
```
以秒为单位设置异步task的轮询时间
```
### changed_when
```
条件表达式，覆盖正常tasks changed状态，告诉ansible此task何时表现为changed状态。
```
### failed_when
```
条件表达式，覆盖正常tasks failed状态，告诉ansible此task何时表现为failed
```
### register
```
设置包含任务状态和模块返回值的变量名
```
### until
```
直到条件满足或者retire次数达到限制停止任务。
```
### retries
```
在until loop中设置失败之前的尝试次数，它只和until条件语句一起使用。
```
### delay
```
在重试之前延时多少秒
```
### local_action
```
类似于delegate_to: localhost 本地机器上执行而非远程机器执行。
```
### loop
```
列表迭代，默认保存每个列表元素到item变量中(可使用loop_control该表)。
```
### loop_control
```
修改/设置 一些loop重的值
```
### with_<lookup_plugin>
```
与loop相同，都是列表迭代。
```
<font color="#dd0000">其他关键字基本与block相同</font>
- delegate_to
- environment
- ignore_errors
- ignore_unreachable
- run_onece
- tags
- timeout
- vars
- when
- notify
