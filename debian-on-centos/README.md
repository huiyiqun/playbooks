Debian on CentOS
================

实验室只有一台机器，所以节约着用。

Pre-Prepare
-----------

* ansible > 2.2
* access to kvm-host

Prepare
-------

你需要写一个inventory文件，这样ansible才能去服务器上执行操作。下面是一个模板：

```
kvm-host ansible_host=alfa.s.tuna.tsinghua.edu.cn ansible_user=<Your Username> ansible_become_pass=<Your Password>
kvm-guest ansible_host=alfa.s.tuna.tsinghua.edu.cn ansible_user=<Your Username> ansible_port=10022 ansible_ssh_pass=<Your Password> ansible_become_pass={{ansible_ssh_pass}} ansible_ssh_extra_args='-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
```

* kvm-host 没什么好说的，写得不对上不去服务器。
* kvm-guest 这一行里的Username和Password可以随便写，安装虚拟机的过程中会用这对用户名密码来建用户
* kvm-guest 这一行里的`ansible_port`写一个alfa上没有被占用的端口就好，在安装中会设置DNAT，将alfa上的`ansible_port`映射到虚拟机的22端口。
* 安装完之后最好还是把密码改了吧。
* kvm-guest 这一行里的`ansible_ssh_extra_args`是因为这台机器是新装的，fingerprint肯定是未知的，而且重装之后还会变，干脆就ignore掉好了，算是个比较脏的hack。

Run it
------

```
$ git clone git@github.com:tuna/playbooks.git
$ cd playbooks/debian-on-centos
$ ansible-playbook -i path/to/inventory deploy.yml
```

之后让你输入个YES，以免不小心把虚拟机重装了。

然后安装就开始了，`TASK [kvm : wait for installation finished]`会卡比较久，如果对安装进度感兴趣可以上host，然后:

```
$ sudo tail -f /tmp/*-serial0.log
```

能看到安装进度。

What has been done?
-------------------

.pia 自己看
