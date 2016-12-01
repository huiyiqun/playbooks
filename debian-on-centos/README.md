Debian on CentOS
================

实验室只有一台机器，所以节约着用。

这个安装脚本最终使用bridge模式macvtap来配置网络，安装过程中使用nat网络。

脚本中假定了kvm-host所在网络中有DHCP(for IPv4)或SLAAC(for IPv6)。

Pre-Prepare
-----------

* ansible >= 2.2
* access to kvm-host

Prepare
-------

你需要写一个inventory文件，这样ansible才能去服务器上执行操作。下面是一个模板：

```
kvm-host ansible_host=<Your host IP> ansible_user=<Your Username> ansible_become_pass=<Your Password>
kvm-guest ansible_user=<Your Username> ansible_ssh_pass=<Your Password> ansible_become_pass={{ansible_ssh_pass}} ansible_ssh_extra_args='-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
```

* kvm-host 没什么好说的，写得不对上不去服务器。
* kvm-guest 这一行里的Username和Password可以随便写，安装虚拟机的过程中会用这对用户名密码来建用户
* 安装完之后最好还是把密码改了吧。
* kvm-guest 这一行里的`ansible_ssh_extra_args`是因为这台机器是新装的，fingerprint肯定是未知的，而且重装之后还会变，干脆就ignore掉好了，算是个比较脏的hack。
* 如果安装完之后还想继续用这个inventory文件，可以把`ansible_host=<Your guest IP>加到 kvm-guest 这一行后面即可，安装过程中不需要这个。

Run it
------

```
$ git clone git@github.com:huiyiqun/playbooks.git
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
