+++
title = "ceph-osd"
description = ""
weight = 2
+++

[osd0]: http://docs.ceph.com/docs/master/rados/configuration/bluestore-config-ref/
[osd1]: https://access.redhat.com/documentation/en-us/red_hat_ceph_storage/3/html/administration_guide/osd-bluestore
[osd2]: http://blog.51cto.com/99cloud/2119884
[osd3]: https://www.slideshare.net/sageweil1/bluestore-a-new-storage-backend-for-ceph-one-year-in
[osd4]: http://lists.ceph.com/pipermail/ceph-users-ceph.com/2017-September/020951.html
[osd5]: http://lists.ceph.com/pipermail/ceph-users-ceph.com/2017-September/020974.html
[osd6]: http://lists.ceph.com/pipermail/ceph-users-ceph.com/2017-September/021037.html
[osd7]: http://lists.ceph.com/pipermail/ceph-users-ceph.com/2017-September/021038.html
[osd8]: https://forum.proxmox.com/threads/ceph-how-to-specifiy-db-device-for-bluestore-osd.44643/
[osd9]: https://pve.proxmox.com/pve-docs/chapter-pveceph.html
[osd10]: http://lists.ceph.com/pipermail/ceph-users-ceph.com/2017-September/020719.html
[osd11]: https://ceph.com/community/new-luminous-bluestore/
[osd12]: https://ceph.com/planet/understanding-bluestore-cephs-new-storage-backend/
[osd13]: http://ourobengr.com/2017/09/understanding-bluestore/
[osd14]: https://github.com/MartinEmrich/kb/blob/master/ceph/Manual-Bluestore.md
[osd15]: https://www.jianshu.com/p/396bf436275a
[osd16]: http://www.jaywaychou.com/
[osd17]: https://cloud.tencent.com/developer/article/1171492
[osd18]: http://xcodest.me/ceph-bluestore-and-ceph-volume.html
[osd19]: http://www.tang-lei.com/2018/06/15/ceph-luminous-%E7%89%88%E6%9C%AC%E9%83%A8%E7%BD%B2-bluestore/
[osd20]: http://blog.wjin.org/posts/ceph-bluestore-bluefs.html

[ceph-volume1]: http://docs.ceph.com/docs/master/ceph-volume/lvm/prepare/#
[ceph-volume2]: http://docs.ceph.com/docs/master/rados/operations/add-or-rm-osds/#
[ceph-volume3]: https://www.suse.com/support/kb/doc/?id=7023110
[ceph-volume4]: https://tracker.ceph.com/issues/23337
[ceph-volume5]: https://github.com/ceph/ceph/pull/20878
[ceph-volume6]: https://github.com/ceph/ceph/pull/20296
[ceph-volume7]: https://tracker.ceph.com/issues/19489

[dm1]: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html-single/dm_multipath/
[dm2]: http://blog.51cto.com/lookingdream/1793606
[dm3]: http://blog.51cto.com/xiaocao13140/2091893
[dm4]: https://access.redhat.com/discussions/2423461
[dm5]: https://access.redhat.com/solutions/227073


# ceph osd 部署
**********
## 资源
**********
1. ceph osd 部署  
[http://docs.ceph.com/docs/master/rados/configuration/bluestore-config-ref/][osd0]  
[redhat administration guide CHAPTER 9. OSD BLUESTORE (TECHNOLOGY PREVIEW)][osd1]  
[开源实践分享：Ceph bluestore部署实践][osd2]  
[BlueStore: A New Storage Backend for Ceph][osd3]  
[[ceph-users] Bluestore OSD_DATA, WAL & DB][osd4]  
[[ceph-users] Bluestore OSD_DATA, WAL & DB][osd5]  
[[ceph-users] Bluestore OSD_DATA, WAL & DB][osd6]  
[[ceph-users] Bluestore OSD_DATA, WAL & DB][osd7]  
[Ceph: How to specifiy DB device for Bluestore OSD][osd8]  
[Manage Ceph Services on Proxmox VE Nodes][osd9]  
[[ceph-users] Bluestore "separate" WAL and DB (and WAL/DB size?)][osd10]  
[https://ceph.com/community/new-luminous-bluestore/][osd11]  
[https://ceph.com/planet/understanding-bluestore-cephs-new-storage-backend/][osd12]  
[http://ourobengr.com/2017/09/understanding-bluestore/][osd13]  
[https://github.com/MartinEmrich/kb/blob/master/ceph/Manual-Bluestore.md][osd14]  
[Ceph 部署（Centos7 + Luminous）][osd15]  
[深入浅出BlueStore的OSD创建与启动][osd16]  
[Luminous下删除和新建OSD的正确姿势][osd17]  
[Ceph bluestore 和 ceph-volume][osd18]  
[ceph-luminous-版本部署-bluestore][osd19]  
[Ceph BlueStore BlueFS][osd20]  
2. ceph-volume  
[http://docs.ceph.com/docs/master/ceph-volume/lvm/prepare/#][ceph-volume1]  
[http://docs.ceph.com/docs/master/rados/operations/add-or-rm-osds/#][ceph-volume2]  
[Using multipath devices for OSDs][ceph-volume3]  
[tracker.ceph.com ceph-volume lvm create fails to detect multipath devices][ceph-volume4]  
[github ceph-volume document multipath support #20878][ceph-volume5]  
[github Allow multipath volumes in ceph-volume. #20296][ceph-volume6]  
[tracker.ceph.com ceph-disk: failing to activate osd with multipath][ceph-volume7]  
3. device mapper multipath  
[https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html-single/dm_multipath/][dm1]  
[Multipath多路径冗余全解析][dm2]  
[Oracle数据库中Linux下多路径使用及大容量硬盘挂载][dm3]  
[redhat pvcreate fails while using multipath device][dm4]  
[redhat Paths to multipath device does not show the newly created partition][dm5]  


## 步骤
**********
0. overview

    ```
    1. add osd
    ```
1. add osd

    ```
    sudo pvcreate /dev/mapper/mpatha
    sudo vgcreate ceph-block-a /dev/mapper/mpatha
    sudo lvcreate -l 100%FREE -n block-a ceph-block-a
    ceph-deploy osd create --data ceph-block-a/block-a server06
    ```
    ```
    [store@server06 ceph-deploy]$ ceph-deploy osd create --data ceph-block-a/block-a server06
    [ceph_deploy.conf][DEBUG ] found configuration file at: /home/store/.cephdeploy.conf
    [ceph_deploy.cli][INFO  ] Invoked (2.0.1): /usr/bin/ceph-deploy osd create --data ceph-block-a/block-a server06
    [ceph_deploy.cli][INFO  ] ceph-deploy options:
    [ceph_deploy.cli][INFO  ]  verbose                       : False
    [ceph_deploy.cli][INFO  ]  bluestore                     : None
    [ceph_deploy.cli][INFO  ]  cd_conf                       : <ceph_deploy.conf.cephdeploy.Conf instance at 0x7fc0a3cf1950>
    [ceph_deploy.cli][INFO  ]  cluster                       : ceph
    [ceph_deploy.cli][INFO  ]  fs_type                       : xfs
    [ceph_deploy.cli][INFO  ]  block_wal                     : None
    [ceph_deploy.cli][INFO  ]  default_release               : False
    [ceph_deploy.cli][INFO  ]  username                      : None
    [ceph_deploy.cli][INFO  ]  journal                       : None
    [ceph_deploy.cli][INFO  ]  subcommand                    : create
    [ceph_deploy.cli][INFO  ]  host                          : server06
    [ceph_deploy.cli][INFO  ]  filestore                     : None
    [ceph_deploy.cli][INFO  ]  func                          : <function osd at 0x7fc0a3f36c80>
    [ceph_deploy.cli][INFO  ]  ceph_conf                     : None
    [ceph_deploy.cli][INFO  ]  zap_disk                      : False
    [ceph_deploy.cli][INFO  ]  data                          : ceph-block-a/block-a
    [ceph_deploy.cli][INFO  ]  block_db                      : None
    [ceph_deploy.cli][INFO  ]  dmcrypt                       : False
    [ceph_deploy.cli][INFO  ]  overwrite_conf                : False
    [ceph_deploy.cli][INFO  ]  dmcrypt_key_dir               : /etc/ceph/dmcrypt-keys
    [ceph_deploy.cli][INFO  ]  quiet                         : False
    [ceph_deploy.cli][INFO  ]  debug                         : False
    [ceph_deploy.osd][DEBUG ] Creating OSD on cluster ceph with data device ceph-block-a/block-a
    [server06][DEBUG ] connection detected need for sudo
    [server06][DEBUG ] connected to host: server06 
    [server06][DEBUG ] detect platform information from remote host
    [server06][DEBUG ] detect machine type
    [server06][DEBUG ] find the location of an executable
    [ceph_deploy.osd][INFO  ] Distro info: CentOS Linux 7.5.1804 Core
    [ceph_deploy.osd][DEBUG ] Deploying osd to server06
    [server06][DEBUG ] write cluster configuration to /etc/ceph/{cluster}.conf
    [server06][DEBUG ] find the location of an executable
    [server06][INFO  ] Running command: sudo /usr/sbin/ceph-volume --cluster ceph lvm create --bluestore --data ceph-block-a/block-a
    [server06][DEBUG ] Running command: /bin/ceph-authtool --gen-print-key
    [server06][DEBUG ] Running command: /bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new 6a3db9e1-82cf-46da-bb3d-b834dfc5ef9e
    [server06][DEBUG ] Running command: /bin/ceph-authtool --gen-print-key
    [server06][DEBUG ] Running command: /bin/mount -t tmpfs tmpfs /var/lib/ceph/osd/ceph-0
    [server06][DEBUG ] Running command: /usr/sbin/restorecon /var/lib/ceph/osd/ceph-0
    [server06][DEBUG ] Running command: /bin/chown -h ceph:ceph /dev/ceph-block-a/block-a
    [server06][DEBUG ] Running command: /bin/chown -R ceph:ceph /dev/dm-4
    [server06][DEBUG ] Running command: /bin/ln -s /dev/ceph-block-a/block-a /var/lib/ceph/osd/ceph-0/block
    [server06][DEBUG ] Running command: /bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring mon getmap -o /var/lib/ceph/osd/ceph-0/activate.monmap
    [server06][DEBUG ]  stderr: got monmap epoch 3
    [server06][DEBUG ] Running command: /bin/ceph-authtool /var/lib/ceph/osd/ceph-0/keyring --create-keyring --name osd.0 --add-key AQDpFSVcy+qQCxAAXP/MY1ac8Pq1V7dNMElzNA==
    [server06][DEBUG ]  stdout: creating /var/lib/ceph/osd/ceph-0/keyring
    [server06][DEBUG ]  stdout: added entity osd.0 auth auth(auid = 18446744073709551615 key=AQDpFSVcy+qQCxAAXP/MY1ac8Pq1V7dNMElzNA== with 0 caps)
    [server06][DEBUG ] Running command: /bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0/keyring
    [server06][DEBUG ] Running command: /bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0/
    [server06][DEBUG ] Running command: /bin/ceph-osd --cluster ceph --osd-objectstore bluestore --mkfs -i 0 --monmap /var/lib/ceph/osd/ceph-0/activate.monmap --keyfile - --osd-data /var/lib/ceph/osd/ceph-0/ --osd-uuid 6a3db9e1-82cf-46da-bb3d-b834dfc5ef9e --setuser ceph --setgroup ceph
    [server06][DEBUG ] --> ceph-volume lvm prepare successful for: ceph-block-a/block-a
    [server06][DEBUG ] Running command: /bin/ceph-bluestore-tool --cluster=ceph prime-osd-dir --dev /dev/ceph-block-a/block-a --path /var/lib/ceph/osd/ceph-0 --no-mon-config
    [server06][DEBUG ] Running command: /bin/ln -snf /dev/ceph-block-a/block-a /var/lib/ceph/osd/ceph-0/block
    [server06][DEBUG ] Running command: /bin/chown -h ceph:ceph /var/lib/ceph/osd/ceph-0/block
    [server06][DEBUG ] Running command: /bin/chown -R ceph:ceph /dev/dm-4
    [server06][DEBUG ] Running command: /bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0
    [server06][DEBUG ] Running command: /bin/systemctl enable ceph-volume@lvm-0-6a3db9e1-82cf-46da-bb3d-b834dfc5ef9e
    [server06][DEBUG ]  stderr: Created symlink from /etc/systemd/system/multi-user.target.wants/ceph-volume@lvm-0-6a3db9e1-82cf-46da-bb3d-b834dfc5ef9e.service to /usr/lib/systemd/system/ceph-volume@.service.
    [server06][DEBUG ] Running command: /bin/systemctl enable --runtime ceph-osd@0
    [server06][DEBUG ]  stderr: Created symlink from /run/systemd/system/ceph-osd.target.wants/ceph-osd@0.service to /usr/lib/systemd/system/ceph-osd@.service.
    [server06][DEBUG ] Running command: /bin/systemctl start ceph-osd@0
    [server06][DEBUG ] --> ceph-volume lvm activate successful for osd ID: 0
    [server06][DEBUG ] --> ceph-volume lvm create successful for: ceph-block-a/block-a
    [server06][INFO  ] checking OSD status...
    [server06][DEBUG ] find the location of an executable
    [server06][INFO  ] Running command: sudo /bin/ceph --cluster=ceph osd stat --format=json
    [ceph_deploy.osd][DEBUG ] Host server06 is now ready for osd use.
    ```
    ```
    [store@server06 ceph-deploy]$ ceph-deploy osd create --data /dev/mapper/mpatha server06
    [ceph_deploy.conf][DEBUG ] found configuration file at: /home/store/.cephdeploy.conf
    [ceph_deploy.cli][INFO  ] Invoked (2.0.1): /usr/bin/ceph-deploy osd create --data /dev/mapper/mpatha server06
    [ceph_deploy.cli][INFO  ] ceph-deploy options:
    [ceph_deploy.cli][INFO  ]  verbose                       : False
    [ceph_deploy.cli][INFO  ]  bluestore                     : None
    [ceph_deploy.cli][INFO  ]  cd_conf                       : <ceph_deploy.conf.cephdeploy.Conf instance at 0x7fef0c5aa950>
    [ceph_deploy.cli][INFO  ]  cluster                       : ceph
    [ceph_deploy.cli][INFO  ]  fs_type                       : xfs
    [ceph_deploy.cli][INFO  ]  block_wal                     : None
    [ceph_deploy.cli][INFO  ]  default_release               : False
    [ceph_deploy.cli][INFO  ]  username                      : None
    [ceph_deploy.cli][INFO  ]  journal                       : None
    [ceph_deploy.cli][INFO  ]  subcommand                    : create
    [ceph_deploy.cli][INFO  ]  host                          : server06
    [ceph_deploy.cli][INFO  ]  filestore                     : None
    [ceph_deploy.cli][INFO  ]  func                          : <function osd at 0x7fef0c7efc80>
    [ceph_deploy.cli][INFO  ]  ceph_conf                     : None
    [ceph_deploy.cli][INFO  ]  zap_disk                      : False
    [ceph_deploy.cli][INFO  ]  data                          : /dev/mapper/mpatha
    [ceph_deploy.cli][INFO  ]  block_db                      : None
    [ceph_deploy.cli][INFO  ]  dmcrypt                       : False
    [ceph_deploy.cli][INFO  ]  overwrite_conf                : False
    [ceph_deploy.cli][INFO  ]  dmcrypt_key_dir               : /etc/ceph/dmcrypt-keys
    [ceph_deploy.cli][INFO  ]  quiet                         : False
    [ceph_deploy.cli][INFO  ]  debug                         : False
    [ceph_deploy.osd][DEBUG ] Creating OSD on cluster ceph with data device /dev/mapper/mpatha
    [server06][DEBUG ] connection detected need for sudo
    [server06][DEBUG ] connected to host: server06 
    [server06][DEBUG ] detect platform information from remote host
    [server06][DEBUG ] detect machine type
    [server06][DEBUG ] find the location of an executable
    [ceph_deploy.osd][INFO  ] Distro info: CentOS Linux 7.5.1804 Core
    [ceph_deploy.osd][DEBUG ] Deploying osd to server06
    [server06][DEBUG ] write cluster configuration to /etc/ceph/{cluster}.conf
    [server06][WARNIN] osd keyring does not exist yet, creating one
    [server06][DEBUG ] create a keyring file
    [server06][DEBUG ] find the location of an executable
    [server06][INFO  ] Running command: sudo /usr/sbin/ceph-volume --cluster ceph lvm create --bluestore --data /dev/mapper/mpatha
    [server06][WARNIN] -->  RuntimeError: Cannot use device (/dev/mapper/mpatha). A vg/lv path or an existing device is needed
    [server06][DEBUG ] Running command: /bin/ceph-authtool --gen-print-key
    [server06][DEBUG ] Running command: /bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new c393d04b-2f1b-4b17-a8de-f4b8b14b9a9d
    [server06][DEBUG ] --> Was unable to complete a new OSD, will rollback changes
    [server06][DEBUG ] Running command: /bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring osd purge-new osd.0 --yes-i-really-mean-it
    [server06][DEBUG ]  stderr: purged osd.0
    [server06][ERROR ] RuntimeError: command returned non-zero exit status: 1
    [ceph_deploy.osd][ERROR ] Failed to execute command: /usr/sbin/ceph-volume --cluster ceph lvm create --bluestore --data /dev/mapper/mpatha
    [ceph_deploy][ERROR ] GenericError: Failed to create 1 OSDs
    ```