# 变量
## vars && vars_files
**设置变量 vars 字典类型， vars_files列表类型**
```
vars:
	key_files: /etc/nginx/ssl/nginx.key
	cert_file: /etc/nginx/ssl/nginx.crt
	conf_file: /etc/nginx/site-available/default
	server_name: localhost

vars_files:
	- nginx.yaml

nginx.yaml
key_files: /etc/nginx/ssl/nginx.key
cert_file: /etc/nginx/ssl/nginx/cert
conf_file: /etc/nginx/site-available/default
server_name: localhost
```
## register
**获取任务执行的返回结果**
```
- name: capture output of whoami command
	command: whoami
	register: login

- name: output task result
	debug: var=login
```

## fact
- ansible_fact
	- **gather_fact: True 执行第一个task之前ansible获取目标主机的详细情况**
	```
	# 查看某台服务器所关联的fact
	ansible server -m setup

	# 获取fact子集
	ansible server -m setup -a "filter=ansible_eth*"
	```
- ansible_local
	- **为某个主机设定fact，将一个或多个文件放置在/etc/ansible/facts.d/目录下(.ini|JSON)**
	```
	/etc/ansible/facts.d/example.fact
	[book]
	name=Ansible: Up and Running
	author=Lorin Hochstein

	# 使用文件中的变量 文件中的变量定义在ansible_local中，ansible_local是一个字典包含文件名example的key
	- name: output local fact
		debug:
			msg:
				"Book name: {{ ansible_local.example.book.name }}"
				
	```
## set_fact
**一般在register之后立即使用，方便变量的使用**
```
- name: Install ntp package
	dnf:
		name: ntp
		state: present
	register: result
- set_fact: 
		rc={{ result.rc }}
```

## 内置变量
|参数|描述|
|---|---|
|hostvars| 字典，key为ansible主机名字，value为所有变量名与相应变量值映射组成的字典|
|inventory_hostname|当前主机被ansible识别的名字|
|group_names|列表，有当前主机所属的所有组群组成|
|groups|字典，key为ansible群组名，value为群组成员主机名的列表，一定包含all、ungrouped两个key|
|play_hosts|列表，成员是当前play涉及的主机的inventory主机名|
|ansible_version|字典,ansible版本信息|
**hostvars、inventory_hostname、groups 最为常用**
- hostvars
	- 应用场景：某一主机运行task可能需要另一主机上定义的变量值,openstack集群中nova配置文件需要数据库的ip
		```
		{{ hostvars['db.domain.tld'].ansible_eth1.ipv4.address }}
		```
- inventory_hostname
	- 应用场景：识别当前主机名，定义别名这里就是那个别名
		```
		hostvars[inventory_hostname]
		```

- groups
	- 应用场景：需要访问一组主机变量时，比如我们配置负载均衡需要一组web的ip
		```
		backend web-backend
		{% for host in groups.web %}
		server {{ host.inventory_hostname }} {{ host.ansible_default_ipv4.address }}:80
		{% endfor %}
		```

## 命令行应用变量
**使用-e参数**
```
# 指定一个变量
ansible-playbook example.yaml -e token=12345
# 指定含有变量的文件
ansible-playbook example.yaml -e @token.yaml
```

## 优先级
1. -e参数
2. local_fact
3. role vars 目录小引用变量
4. vars vars_files 引用变量
3. inventory文件或者yaml文件定义的主机或群组便利那个
4. Fact
5. role中的defaults/main.yaml
