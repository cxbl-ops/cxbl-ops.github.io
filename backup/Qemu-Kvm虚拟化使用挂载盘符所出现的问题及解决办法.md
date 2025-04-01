### 问题描述

我想要使用qemu-kvm创建Windows虚拟机时，使用挂载的盘（在Windows中为D盘）作为Windwos的安装位置，但是出现了如下报错:



```shell

无法完成安装：'Cannot access storage file '/media/vir/qc/win10.qcow2' (as uid:64055, gid:109): 权限不够'
Traceback (most recent call last):
  File "/usr/share/virt-manager/virtManager/asyncjob.py", line 72, in cb_wrapper
    callback(asyncjob, *args, **kwargs)
  File "/usr/share/virt-manager/virtManager/createvm.py", line 2008, in _do_async_install
    installer.start_install(guest, meter=meter)
  File "/usr/share/virt-manager/virtinst/install/installer.py", line 695, in start_install
    domain = self._create_guest(
  File "/usr/share/virt-manager/virtinst/install/installer.py", line 637, in _create_guest
    domain = self.conn.createXML(initial_xml or final_xml, 0)
  File "/usr/lib/python3/dist-packages/libvirt.py", line 4400, in createXML
    raise libvirtError('virDomainCreateXML() failed')
libvirt.libvirtError: Cannot access storage file '/media/vir/qc/win10.qcow2' (as uid:64055, gid:109): 权限不够
```
### 问题解决

这是常见的 KVM Libvirt 错误之一。此错误通常在更改 Libvirt 默认存储目录的路径]后发生。

* 编辑`/etc/libvirt/qemu.conf`文件：


  ```shell

  sudo nano /etc/libvirt/qemu.conf

  ```

* 找到`user`和`group`指令,在对应位置去掉注释后改为如下设置：

  ```shell
  # Some examples of valid values are:
  #
  #       user = "qemu"   # A user named "qemu"
  #       user = "+0"     # Super user (uid=0
  #       user = "100"    # A user named "100" or a user with uid=100
  #
  user = "vir"  #用户名
  # The group for QEMU processes run by the system instance. It can be
  # specified in a similar way to user.
  group = "libvirt" # 用户组
  ```
* 重新启动Libvirt
  ```shell

  systemctl restart libvirtd
  
  ```
  至此，问题解决。