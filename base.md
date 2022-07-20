# 公钥认证
**known_hosts文件中记录主机密钥,ansible默认会进行认证**
- 首次登陆机器会提示认证信息yes/no让用户选择
- 重建机器密钥改变会弹出错误信息，需要重新认证
```
关闭密钥认证方法
1. export ANSIBLE_HOST_KEY_CHECKING=False
2. /etc/ansible/ansible.cfg or ~/.ansible.cfg 文件中添加
[default]
host_key_checking = False
```
# inventory
## 相似主机简写
```
[compute]
com[01-50].domain.tld
db-[a-f].domain.tld
```
## 主机、组变量(ini 格式)
```
# com01主机拥有特殊变量
[compute]
com01.domain.tld ansible_connection=ssh ansible_ssh_user=root
com02.domain.tld 

[controller]
ctl[01-03].domain.tld

# controller组内主机都可使用下列变量
[controller:vars]
internal_vip_addr=192.168.111.33
external_vip_addr=172.16.111.33
```

## 文件定义主机、组变量(yaml格式)
- /etc/ansible/group_vars/[file or directory]
- /etc/ansible/host_vars/[file or directory]

**group_vars/ 和 host_vars/ 目录可放在 inventory 目录下,或是 playbook 目录下. 如果两个目录下都存在,那么 playbook 目录下的配置会覆盖 inventory 目录的配置**
```
# file(文件名为主机名或组名,内部定义变量)
/etc/ansible/group_vars/controller
/etc/ansible/host_vars/com01.domain.tld

# directory(目录名为主机名或组名,目录文件中定义变量)
/etc/ansible/group_vars/controller/db_setting
/etc/ansible/group_vars/controller/cluster_setting
/etc/ansible/host_vars/com01.domain.tld/hostname_setting
```
## inventory 内部变量
| 变量名称 | 变量说明  |
|--|--|
| ansible_ssh_host  | 要连接的远程主机名  |
| ansible_ssh_port  | ssh端口号 |
| ansible_ssh_user  | ssh用户名 |
| ansible_ssh_pass  | ssh密码 |
| ansible_sudo_pass | sudo密码 |
| ansible_sudo_exe | sudo命令路径1.8及以上版本 |
| ansible_connection | 与主机的连接类型.比如:local, ssh 或者 paramiko. |
| ansible_shell_type | 目标系统的shell类型.默认情况下,命令的执行使用 'sh' 语法,可设置为 'csh' 或 'fish'. |
| ansible_python_interpreter | 系统中有多个 Python, 或者命令路径不是"/usr/bin/python" |
| ansible_ssh_private_key_file | ssh 使用的私钥文件 |
# 通配符使用选择执行目标主机

## 选中inventory中所有机器
```
all
*
```
## 组之间的与 或 非关系
```
# 非(选中在隶属于controller组但不在compute组的机器)
controller:!compute

# 与(选中即隶属于controller组又隶属于compute组的机器)
controller:&compute

# 或(选中隶属于controller组和隶属于compute组的所有机器)
controller:compute

```
## 变量传递实现动态group选择
**ansible-playbook通过 “-e” 参数可以实现**
```
webservers:!{{excluded}}:&{{required}}
```

## 正则使用
```
~(web|db).*\.domain\.tld

```
## limit标记
**limit标记来添加排除条件,/usr/bin/ansible or /usr/bin/ansible-playbook都支持**

```
# 读取名称
ansible-playbook site.yml --limit retry
# 读取文件
ansible-playbook site.yml --limit @retry.txt
```


# 配置文件

- ansible_managed: 代表一段字符串,可以插入ansible生成的模版中{{ ansible_managed }},设置打印哪个用户 什么时间修改了模版文件内容
	- ansible_managed = Ansible managed: {file} modified on %Y-%m-%d %H:%M:%S by {uid} on {host}
- ask_pass：是否询问密码,密钥认证必须要修改此项
	- ask_pass = False
- ask_sudo_pass: sudo之前是否询问密码
	- ask_sudo_pass = False
- command_warnings: 是否提示警告信息, shell执行某些命令可以被模块替代会发出警告信息
	- command_warnings = False
- deprecation_warnings：不建议的警告信息，新版本会被淘汰的遗留特性
	- deprecation_warnings = False
- display_skipped_hosts: 是否现实跳过任务的状态
	- display_skipped_hosts = True
- error_on_undefined_vars: 引用变量名错误导致ansible执行失败
	- error_on_undefined_vars = True
- forks: 并行的进程数
	- _forks = 10
- gathering: 控制是否默认facts收集,implicit每次play都收集除非定义gather_facts: False,explicit与之相反
	- gathering = implicit
- host_key_checking：是否验证公钥
	- host_key_checking = False
- inventory: 默认库文件位置
	- inventory = /etc/ansible/hosts
- log_path: 日志文件目录
	- log_path = /var/log/ansible.log
- patten: 要通信的默认主机组，playbook需要定义,默认是*
	- hosts = * 
- remote_port: 默认远程ssh端口
	- remote_port = 22
- remote_tmp: 远程执行模块默认目录
	- remote_tmp = $HOME/.ansible/tmp
- remote_user: /usr/bin/ansible-playbook链接的默认用户名.
	- remote_user = root
- roles_path: 查找role的路径,多余的路径可以用冒号分隔
	- roles_path = /opt/mysite/roles	
- timeout: ssh连接的超时时间
  - timeout = 10
- ssh_args：传递ssh参数不使用默认值
	- ssh_args = -o ControlMaster=auto -o ControlPersist=60s
- scp_if_ssh: 操作没有开启sftp的远程主机时需要设置True,scp代替sftp传递文件
	- scp_if_ssh = False
