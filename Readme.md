# 个人常用 ansible 脚本

## 快速开始

``` bash
# 安装依赖
$ ansible-galaxy install -r requirements.yml

# 安装 playbook
$ ansible-playbook -i hosts git.yml
```

## Q&A

### 如何得知所有的目标机器是否可达

``` bash
$ ansibel all -m ping
```

### 如何得知所有的目标机器的发行版

``` bash
$ ansible all -m setup
```

### 如何在所有目标机器上执行命令

``` bash
$ ansible all -a pwd
```

### 一个 task 如何根据条件执行

添加参数 `when` 作为分支条件

``` bash
# 当在国内时，选择阿里云镜像
- name: add docker repo -> aliyun
  shell: yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  when: ansible_date_time.tz_offset == "+0800"
  args:
    creates: /etc/yum.repos.d/docker-ce.repo 
```

### 如果一个 task 执行错误，如何忽略错误使它继续执行下去

添加参数 `ignore_errors: true` 忽略错误


### 如何根据一个 task 的执行结果来决定是否执行其它 task

使用 `register` 来监听当前任务的执行情况，使用 `when` 设定条件

### 为什么使用 git，file 等模块比直接执行 shell 要好些

git，file 能够更好地满足幂等性

## ansible 注意点

+ 在使用相关 ansible 模块前请先确认是否满足 requirements
