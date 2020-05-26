# 个人常用 ansible 脚本

## 快速开始

``` bash
# 安装依赖
$ ansible-galaxy install -r requirements.yml

# 安装某一个 playbook
$ ansible-playbook -i hosts git.yml

# 指定某一台测试服务器，一键安装
$ ansible-playbook -i hosts --limit=op prepare.yml git.yml tmux.yml vim.yml language.yml zsh.yml
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

### 如何根据条件执行 Task

``` bash
# 当在国内时，选择阿里云镜像
- name: add docker repo -> aliyun
  shell: yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  when: ansible_date_time.tz_offset == "+0800"
  args:
    creates: /etc/yum.repos.d/docker-ce.repo 
```

### 如何忽略错误使 Task 继续执行下去

`ignore_errors: true`


### 如何根据一个 task 的执行结果来决定是否执行其它 task

使用 `register` 来监听当前任务的执行情况，使用 `when` 设定条件

### 为什么使用 git，file 等模块比直接执行 shell 要好些

git，file 能够更好地满足幂等性
