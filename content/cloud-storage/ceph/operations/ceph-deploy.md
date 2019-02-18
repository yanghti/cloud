+++
title = "ceph-deploy"
description = ""
weight = 3
+++

# ceph deploy
**********
## 资源
**********
1. ceph deploy

## 步骤
**********
0. overview

    ```
    1. mon
    2. mgr
    3. osd
    ```
1. mon

    ```
    [ceph-sh-admin@ceph-sh-admin ceph-deploy]$ ceph-deploy mon create-initial
    [ceph_deploy.conf][DEBUG ] found configuration file at: /home/ceph-sh-admin/.cephdeploy.conf
    [ceph_deploy.cli][INFO  ] Invoked (2.0.1): /usr/bin/ceph-deploy mon create-initial
    [ceph_deploy.cli][INFO  ] ceph-deploy options:
    [ceph_deploy.cli][INFO  ]  username                      : None
    [ceph_deploy.cli][INFO  ]  verbose                       : False
    [ceph_deploy.cli][INFO  ]  overwrite_conf                : False
    [ceph_deploy.cli][INFO  ]  subcommand                    : create-initial
    [ceph_deploy.cli][INFO  ]  quiet                         : False
    [ceph_deploy.cli][INFO  ]  cd_conf                       : <ceph_deploy.conf.cephdeploy.Conf instance at 0x7f203c9bacb0>
    [ceph_deploy.cli][INFO  ]  cluster                       : ceph
    [ceph_deploy.cli][INFO  ]  func                          : <function mon at 0x7f203ce33398>
    [ceph_deploy.cli][INFO  ]  ceph_conf                     : None
    [ceph_deploy.cli][INFO  ]  default_release               : False
    [ceph_deploy.cli][INFO  ]  keyrings                      : None
    [ceph_deploy.mon][DEBUG ] Deploying mon, cluster ceph hosts ceph-sh-node1 ceph-sh-node2 ceph-sh-node3
    [ceph_deploy.mon][DEBUG ] detecting platform for host ceph-sh-node1 ...
    [ceph-sh-node1][DEBUG ] connection detected need for sudo
    [ceph-sh-node1][DEBUG ] connected to host: ceph-sh-node1 
    [ceph-sh-node1][DEBUG ] detect platform information from remote host
    [ceph-sh-node1][DEBUG ] detect machine type
    [ceph-sh-node1][DEBUG ] find the location of an executable
    [ceph_deploy.mon][INFO  ] distro info: CentOS Linux 7.5.1804 Core
    [ceph-sh-node1][DEBUG ] determining if provided host has same hostname in remote
    [ceph-sh-node1][DEBUG ] get remote short hostname
    [ceph-sh-node1][DEBUG ] deploying mon to ceph-sh-node1
    [ceph-sh-node1][DEBUG ] get remote short hostname
    [ceph-sh-node1][DEBUG ] remote hostname: ceph-sh-node1
    [ceph-sh-node1][DEBUG ] write cluster configuration to /etc/ceph/{cluster}.conf
    [ceph-sh-node1][DEBUG ] create the mon path if it does not exist
    [ceph-sh-node1][DEBUG ] checking for done path: /var/lib/ceph/mon/ceph-ceph-sh-node1/done
    [ceph-sh-node1][DEBUG ] done path does not exist: /var/lib/ceph/mon/ceph-ceph-sh-node1/done
    [ceph-sh-node1][INFO  ] creating keyring file: /var/lib/ceph/tmp/ceph-ceph-sh-node1.mon.keyring
    [ceph-sh-node1][DEBUG ] create the monitor keyring file
    [ceph-sh-node1][INFO  ] Running command: sudo ceph-mon --cluster ceph --mkfs -i ceph-sh-node1 --keyring /var/lib/ceph/tmp/ceph-ceph-sh-node1.mon.keyring --setuser 167 --setgroup 167
    [ceph-sh-node1][INFO  ] unlinking keyring file /var/lib/ceph/tmp/ceph-ceph-sh-node1.mon.keyring
    [ceph-sh-node1][DEBUG ] create a done file to avoid re-doing the mon deployment
    [ceph-sh-node1][DEBUG ] create the init path if it does not exist
    [ceph-sh-node1][INFO  ] Running command: sudo systemctl enable ceph.target
    [ceph-sh-node1][INFO  ] Running command: sudo systemctl enable ceph-mon@ceph-sh-node1
    [ceph-sh-node1][WARNIN] Created symlink from /etc/systemd/system/ceph-mon.target.wants/ceph-mon@ceph-sh-node1.service to /usr/lib/systemd/system/ceph-mon@.service.
    [ceph-sh-node1][INFO  ] Running command: sudo systemctl start ceph-mon@ceph-sh-node1
    [ceph-sh-node1][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node1.asok mon_status
    [ceph-sh-node1][DEBUG ] ********************************************************************************
    [ceph-sh-node1][DEBUG ] status for monitor: mon.ceph-sh-node1
    [ceph-sh-node1][DEBUG ] {
    [ceph-sh-node1][DEBUG ]   "election_epoch": 0, 
    [ceph-sh-node1][DEBUG ]   "extra_probe_peers": [
    [ceph-sh-node1][DEBUG ]     "192.168.59.202:6789/0", 
    [ceph-sh-node1][DEBUG ]     "192.168.59.203:6789/0"
    [ceph-sh-node1][DEBUG ]   ], 
    [ceph-sh-node1][DEBUG ]   "feature_map": {
    [ceph-sh-node1][DEBUG ]     "mon": [
    [ceph-sh-node1][DEBUG ]       {
    [ceph-sh-node1][DEBUG ]         "features": "0x3ffddff8ffa4fffb", 
    [ceph-sh-node1][DEBUG ]         "num": 1, 
    [ceph-sh-node1][DEBUG ]         "release": "luminous"
    [ceph-sh-node1][DEBUG ]       }
    [ceph-sh-node1][DEBUG ]     ]
    [ceph-sh-node1][DEBUG ]   }, 
    [ceph-sh-node1][DEBUG ]   "features": {
    [ceph-sh-node1][DEBUG ]     "quorum_con": "0", 
    [ceph-sh-node1][DEBUG ]     "quorum_mon": [], 
    [ceph-sh-node1][DEBUG ]     "required_con": "0", 
    [ceph-sh-node1][DEBUG ]     "required_mon": []
    [ceph-sh-node1][DEBUG ]   }, 
    [ceph-sh-node1][DEBUG ]   "monmap": {
    [ceph-sh-node1][DEBUG ]     "created": "2019-02-13 22:05:49.949727", 
    [ceph-sh-node1][DEBUG ]     "epoch": 0, 
    [ceph-sh-node1][DEBUG ]     "features": {
    [ceph-sh-node1][DEBUG ]       "optional": [], 
    [ceph-sh-node1][DEBUG ]       "persistent": []
    [ceph-sh-node1][DEBUG ]     }, 
    [ceph-sh-node1][DEBUG ]     "fsid": "6e6d08a5-c4e9-43ae-90fa-166677400761", 
    [ceph-sh-node1][DEBUG ]     "modified": "2019-02-13 22:05:49.949727", 
    [ceph-sh-node1][DEBUG ]     "mons": [
    [ceph-sh-node1][DEBUG ]       {
    [ceph-sh-node1][DEBUG ]         "addr": "192.168.59.201:6789/0", 
    [ceph-sh-node1][DEBUG ]         "name": "ceph-sh-node1", 
    [ceph-sh-node1][DEBUG ]         "public_addr": "192.168.59.201:6789/0", 
    [ceph-sh-node1][DEBUG ]         "rank": 0
    [ceph-sh-node1][DEBUG ]       }, 
    [ceph-sh-node1][DEBUG ]       {
    [ceph-sh-node1][DEBUG ]         "addr": "0.0.0.0:0/1", 
    [ceph-sh-node1][DEBUG ]         "name": "ceph-sh-node2", 
    [ceph-sh-node1][DEBUG ]         "public_addr": "0.0.0.0:0/1", 
    [ceph-sh-node1][DEBUG ]         "rank": 1
    [ceph-sh-node1][DEBUG ]       }, 
    [ceph-sh-node1][DEBUG ]       {
    [ceph-sh-node1][DEBUG ]         "addr": "0.0.0.0:0/2", 
    [ceph-sh-node1][DEBUG ]         "name": "ceph-sh-node3", 
    [ceph-sh-node1][DEBUG ]         "public_addr": "0.0.0.0:0/2", 
    [ceph-sh-node1][DEBUG ]         "rank": 2
    [ceph-sh-node1][DEBUG ]       }
    [ceph-sh-node1][DEBUG ]     ]
    [ceph-sh-node1][DEBUG ]   }, 
    [ceph-sh-node1][DEBUG ]   "name": "ceph-sh-node1", 
    [ceph-sh-node1][DEBUG ]   "outside_quorum": [
    [ceph-sh-node1][DEBUG ]     "ceph-sh-node1"
    [ceph-sh-node1][DEBUG ]   ], 
    [ceph-sh-node1][DEBUG ]   "quorum": [], 
    [ceph-sh-node1][DEBUG ]   "rank": 0, 
    [ceph-sh-node1][DEBUG ]   "state": "probing", 
    [ceph-sh-node1][DEBUG ]   "sync_provider": []
    [ceph-sh-node1][DEBUG ] }
    [ceph-sh-node1][DEBUG ] ********************************************************************************
    [ceph-sh-node1][INFO  ] monitor: mon.ceph-sh-node1 is running
    [ceph-sh-node1][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node1.asok mon_status
    [ceph_deploy.mon][DEBUG ] detecting platform for host ceph-sh-node2 ...
    [ceph-sh-node2][DEBUG ] connection detected need for sudo
    [ceph-sh-node2][DEBUG ] connected to host: ceph-sh-node2 
    [ceph-sh-node2][DEBUG ] detect platform information from remote host
    [ceph-sh-node2][DEBUG ] detect machine type
    [ceph-sh-node2][DEBUG ] find the location of an executable
    [ceph_deploy.mon][INFO  ] distro info: CentOS Linux 7.5.1804 Core
    [ceph-sh-node2][DEBUG ] determining if provided host has same hostname in remote
    [ceph-sh-node2][DEBUG ] get remote short hostname
    [ceph-sh-node2][DEBUG ] deploying mon to ceph-sh-node2
    [ceph-sh-node2][DEBUG ] get remote short hostname
    [ceph-sh-node2][DEBUG ] remote hostname: ceph-sh-node2
    [ceph-sh-node2][DEBUG ] write cluster configuration to /etc/ceph/{cluster}.conf
    [ceph-sh-node2][DEBUG ] create the mon path if it does not exist
    [ceph-sh-node2][DEBUG ] checking for done path: /var/lib/ceph/mon/ceph-ceph-sh-node2/done
    [ceph-sh-node2][DEBUG ] done path does not exist: /var/lib/ceph/mon/ceph-ceph-sh-node2/done
    [ceph-sh-node2][INFO  ] creating keyring file: /var/lib/ceph/tmp/ceph-ceph-sh-node2.mon.keyring
    [ceph-sh-node2][DEBUG ] create the monitor keyring file
    [ceph-sh-node2][INFO  ] Running command: sudo ceph-mon --cluster ceph --mkfs -i ceph-sh-node2 --keyring /var/lib/ceph/tmp/ceph-ceph-sh-node2.mon.keyring --setuser 167 --setgroup 167
    [ceph-sh-node2][INFO  ] unlinking keyring file /var/lib/ceph/tmp/ceph-ceph-sh-node2.mon.keyring
    [ceph-sh-node2][DEBUG ] create a done file to avoid re-doing the mon deployment
    [ceph-sh-node2][DEBUG ] create the init path if it does not exist
    [ceph-sh-node2][INFO  ] Running command: sudo systemctl enable ceph.target
    [ceph-sh-node2][INFO  ] Running command: sudo systemctl enable ceph-mon@ceph-sh-node2
    [ceph-sh-node2][WARNIN] Created symlink from /etc/systemd/system/ceph-mon.target.wants/ceph-mon@ceph-sh-node2.service to /usr/lib/systemd/system/ceph-mon@.service.
    [ceph-sh-node2][INFO  ] Running command: sudo systemctl start ceph-mon@ceph-sh-node2
    [ceph-sh-node2][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node2.asok mon_status
    [ceph-sh-node2][DEBUG ] ********************************************************************************
    [ceph-sh-node2][DEBUG ] status for monitor: mon.ceph-sh-node2
    [ceph-sh-node2][DEBUG ] {
    [ceph-sh-node2][DEBUG ]   "election_epoch": 1, 
    [ceph-sh-node2][DEBUG ]   "extra_probe_peers": [
    [ceph-sh-node2][DEBUG ]     "192.168.59.201:6789/0", 
    [ceph-sh-node2][DEBUG ]     "192.168.59.203:6789/0"
    [ceph-sh-node2][DEBUG ]   ], 
    [ceph-sh-node2][DEBUG ]   "feature_map": {
    [ceph-sh-node2][DEBUG ]     "mon": [
    [ceph-sh-node2][DEBUG ]       {
    [ceph-sh-node2][DEBUG ]         "features": "0x3ffddff8ffa4fffb", 
    [ceph-sh-node2][DEBUG ]         "num": 1, 
    [ceph-sh-node2][DEBUG ]         "release": "luminous"
    [ceph-sh-node2][DEBUG ]       }
    [ceph-sh-node2][DEBUG ]     ]
    [ceph-sh-node2][DEBUG ]   }, 
    [ceph-sh-node2][DEBUG ]   "features": {
    [ceph-sh-node2][DEBUG ]     "quorum_con": "0", 
    [ceph-sh-node2][DEBUG ]     "quorum_mon": [], 
    [ceph-sh-node2][DEBUG ]     "required_con": "0", 
    [ceph-sh-node2][DEBUG ]     "required_mon": []
    [ceph-sh-node2][DEBUG ]   }, 
    [ceph-sh-node2][DEBUG ]   "monmap": {
    [ceph-sh-node2][DEBUG ]     "created": "2019-02-13 22:05:54.402957", 
    [ceph-sh-node2][DEBUG ]     "epoch": 0, 
    [ceph-sh-node2][DEBUG ]     "features": {
    [ceph-sh-node2][DEBUG ]       "optional": [], 
    [ceph-sh-node2][DEBUG ]       "persistent": []
    [ceph-sh-node2][DEBUG ]     }, 
    [ceph-sh-node2][DEBUG ]     "fsid": "6e6d08a5-c4e9-43ae-90fa-166677400761", 
    [ceph-sh-node2][DEBUG ]     "modified": "2019-02-13 22:05:54.402957", 
    [ceph-sh-node2][DEBUG ]     "mons": [
    [ceph-sh-node2][DEBUG ]       {
    [ceph-sh-node2][DEBUG ]         "addr": "192.168.59.201:6789/0", 
    [ceph-sh-node2][DEBUG ]         "name": "ceph-sh-node1", 
    [ceph-sh-node2][DEBUG ]         "public_addr": "192.168.59.201:6789/0", 
    [ceph-sh-node2][DEBUG ]         "rank": 0
    [ceph-sh-node2][DEBUG ]       }, 
    [ceph-sh-node2][DEBUG ]       {
    [ceph-sh-node2][DEBUG ]         "addr": "192.168.59.202:6789/0", 
    [ceph-sh-node2][DEBUG ]         "name": "ceph-sh-node2", 
    [ceph-sh-node2][DEBUG ]         "public_addr": "192.168.59.202:6789/0", 
    [ceph-sh-node2][DEBUG ]         "rank": 1
    [ceph-sh-node2][DEBUG ]       }, 
    [ceph-sh-node2][DEBUG ]       {
    [ceph-sh-node2][DEBUG ]         "addr": "0.0.0.0:0/2", 
    [ceph-sh-node2][DEBUG ]         "name": "ceph-sh-node3", 
    [ceph-sh-node2][DEBUG ]         "public_addr": "0.0.0.0:0/2", 
    [ceph-sh-node2][DEBUG ]         "rank": 2
    [ceph-sh-node2][DEBUG ]       }
    [ceph-sh-node2][DEBUG ]     ]
    [ceph-sh-node2][DEBUG ]   }, 
    [ceph-sh-node2][DEBUG ]   "name": "ceph-sh-node2", 
    [ceph-sh-node2][DEBUG ]   "outside_quorum": [], 
    [ceph-sh-node2][DEBUG ]   "quorum": [], 
    [ceph-sh-node2][DEBUG ]   "rank": 1, 
    [ceph-sh-node2][DEBUG ]   "state": "electing", 
    [ceph-sh-node2][DEBUG ]   "sync_provider": []
    [ceph-sh-node2][DEBUG ] }
    [ceph-sh-node2][DEBUG ] ********************************************************************************
    [ceph-sh-node2][INFO  ] monitor: mon.ceph-sh-node2 is running
    [ceph-sh-node2][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node2.asok mon_status
    [ceph_deploy.mon][DEBUG ] detecting platform for host ceph-sh-node3 ...
    [ceph-sh-node3][DEBUG ] connection detected need for sudo
    [ceph-sh-node3][DEBUG ] connected to host: ceph-sh-node3 
    [ceph-sh-node3][DEBUG ] detect platform information from remote host
    [ceph-sh-node3][DEBUG ] detect machine type
    [ceph-sh-node3][DEBUG ] find the location of an executable
    [ceph_deploy.mon][INFO  ] distro info: CentOS Linux 7.5.1804 Core
    [ceph-sh-node3][DEBUG ] determining if provided host has same hostname in remote
    [ceph-sh-node3][DEBUG ] get remote short hostname
    [ceph-sh-node3][DEBUG ] deploying mon to ceph-sh-node3
    [ceph-sh-node3][DEBUG ] get remote short hostname
    [ceph-sh-node3][DEBUG ] remote hostname: ceph-sh-node3
    [ceph-sh-node3][DEBUG ] write cluster configuration to /etc/ceph/{cluster}.conf
    [ceph-sh-node3][DEBUG ] create the mon path if it does not exist
    [ceph-sh-node3][DEBUG ] checking for done path: /var/lib/ceph/mon/ceph-ceph-sh-node3/done
    [ceph-sh-node3][DEBUG ] done path does not exist: /var/lib/ceph/mon/ceph-ceph-sh-node3/done
    [ceph-sh-node3][INFO  ] creating keyring file: /var/lib/ceph/tmp/ceph-ceph-sh-node3.mon.keyring
    [ceph-sh-node3][DEBUG ] create the monitor keyring file
    [ceph-sh-node3][INFO  ] Running command: sudo ceph-mon --cluster ceph --mkfs -i ceph-sh-node3 --keyring /var/lib/ceph/tmp/ceph-ceph-sh-node3.mon.keyring --setuser 167 --setgroup 167
    [ceph-sh-node3][INFO  ] unlinking keyring file /var/lib/ceph/tmp/ceph-ceph-sh-node3.mon.keyring
    [ceph-sh-node3][DEBUG ] create a done file to avoid re-doing the mon deployment
    [ceph-sh-node3][DEBUG ] create the init path if it does not exist
    [ceph-sh-node3][INFO  ] Running command: sudo systemctl enable ceph.target
    [ceph-sh-node3][INFO  ] Running command: sudo systemctl enable ceph-mon@ceph-sh-node3
    [ceph-sh-node3][WARNIN] Created symlink from /etc/systemd/system/ceph-mon.target.wants/ceph-mon@ceph-sh-node3.service to /usr/lib/systemd/system/ceph-mon@.service.
    [ceph-sh-node3][INFO  ] Running command: sudo systemctl start ceph-mon@ceph-sh-node3
    [ceph-sh-node3][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node3.asok mon_status
    [ceph-sh-node3][DEBUG ] ********************************************************************************
    [ceph-sh-node3][DEBUG ] status for monitor: mon.ceph-sh-node3
    [ceph-sh-node3][DEBUG ] {
    [ceph-sh-node3][DEBUG ]   "election_epoch": 1, 
    [ceph-sh-node3][DEBUG ]   "extra_probe_peers": [
    [ceph-sh-node3][DEBUG ]     "192.168.59.201:6789/0", 
    [ceph-sh-node3][DEBUG ]     "192.168.59.202:6789/0"
    [ceph-sh-node3][DEBUG ]   ], 
    [ceph-sh-node3][DEBUG ]   "feature_map": {
    [ceph-sh-node3][DEBUG ]     "mon": [
    [ceph-sh-node3][DEBUG ]       {
    [ceph-sh-node3][DEBUG ]         "features": "0x3ffddff8ffa4fffb", 
    [ceph-sh-node3][DEBUG ]         "num": 1, 
    [ceph-sh-node3][DEBUG ]         "release": "luminous"
    [ceph-sh-node3][DEBUG ]       }
    [ceph-sh-node3][DEBUG ]     ]
    [ceph-sh-node3][DEBUG ]   }, 
    [ceph-sh-node3][DEBUG ]   "features": {
    [ceph-sh-node3][DEBUG ]     "quorum_con": "0", 
    [ceph-sh-node3][DEBUG ]     "quorum_mon": [], 
    [ceph-sh-node3][DEBUG ]     "required_con": "0", 
    [ceph-sh-node3][DEBUG ]     "required_mon": []
    [ceph-sh-node3][DEBUG ]   }, 
    [ceph-sh-node3][DEBUG ]   "monmap": {
    [ceph-sh-node3][DEBUG ]     "created": "2019-02-13 22:05:58.789759", 
    [ceph-sh-node3][DEBUG ]     "epoch": 0, 
    [ceph-sh-node3][DEBUG ]     "features": {
    [ceph-sh-node3][DEBUG ]       "optional": [], 
    [ceph-sh-node3][DEBUG ]       "persistent": []
    [ceph-sh-node3][DEBUG ]     }, 
    [ceph-sh-node3][DEBUG ]     "fsid": "6e6d08a5-c4e9-43ae-90fa-166677400761", 
    [ceph-sh-node3][DEBUG ]     "modified": "2019-02-13 22:05:58.789759", 
    [ceph-sh-node3][DEBUG ]     "mons": [
    [ceph-sh-node3][DEBUG ]       {
    [ceph-sh-node3][DEBUG ]         "addr": "192.168.59.202:6789/0", 
    [ceph-sh-node3][DEBUG ]         "name": "ceph-sh-node2", 
    [ceph-sh-node3][DEBUG ]         "public_addr": "192.168.59.202:6789/0", 
    [ceph-sh-node3][DEBUG ]         "rank": 0
    [ceph-sh-node3][DEBUG ]       }, 
    [ceph-sh-node3][DEBUG ]       {
    [ceph-sh-node3][DEBUG ]         "addr": "192.168.59.203:6789/0", 
    [ceph-sh-node3][DEBUG ]         "name": "ceph-sh-node3", 
    [ceph-sh-node3][DEBUG ]         "public_addr": "192.168.59.203:6789/0", 
    [ceph-sh-node3][DEBUG ]         "rank": 1
    [ceph-sh-node3][DEBUG ]       }, 
    [ceph-sh-node3][DEBUG ]       {
    [ceph-sh-node3][DEBUG ]         "addr": "0.0.0.0:0/1", 
    [ceph-sh-node3][DEBUG ]         "name": "ceph-sh-node1", 
    [ceph-sh-node3][DEBUG ]         "public_addr": "0.0.0.0:0/1", 
    [ceph-sh-node3][DEBUG ]         "rank": 2
    [ceph-sh-node3][DEBUG ]       }
    [ceph-sh-node3][DEBUG ]     ]
    [ceph-sh-node3][DEBUG ]   }, 
    [ceph-sh-node3][DEBUG ]   "name": "ceph-sh-node3", 
    [ceph-sh-node3][DEBUG ]   "outside_quorum": [], 
    [ceph-sh-node3][DEBUG ]   "quorum": [], 
    [ceph-sh-node3][DEBUG ]   "rank": 1, 
    [ceph-sh-node3][DEBUG ]   "state": "electing", 
    [ceph-sh-node3][DEBUG ]   "sync_provider": []
    [ceph-sh-node3][DEBUG ] }
    [ceph-sh-node3][DEBUG ] ********************************************************************************
    [ceph-sh-node3][INFO  ] monitor: mon.ceph-sh-node3 is running
    [ceph-sh-node3][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node3.asok mon_status
    [ceph_deploy.mon][INFO  ] processing monitor mon.ceph-sh-node1
    [ceph-sh-node1][DEBUG ] connection detected need for sudo
    [ceph-sh-node1][DEBUG ] connected to host: ceph-sh-node1 
    [ceph-sh-node1][DEBUG ] detect platform information from remote host
    [ceph-sh-node1][DEBUG ] detect machine type
    [ceph-sh-node1][DEBUG ] find the location of an executable
    [ceph-sh-node1][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node1.asok mon_status
    [ceph_deploy.mon][WARNIN] mon.ceph-sh-node1 monitor is not yet in quorum, tries left: 5
    [ceph_deploy.mon][WARNIN] waiting 5 seconds before retrying
    [ceph-sh-node1][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node1.asok mon_status
    [ceph_deploy.mon][INFO  ] mon.ceph-sh-node1 monitor has reached quorum!
    [ceph_deploy.mon][INFO  ] processing monitor mon.ceph-sh-node2
    [ceph-sh-node2][DEBUG ] connection detected need for sudo
    [ceph-sh-node2][DEBUG ] connected to host: ceph-sh-node2 
    [ceph-sh-node2][DEBUG ] detect platform information from remote host
    [ceph-sh-node2][DEBUG ] detect machine type
    [ceph-sh-node2][DEBUG ] find the location of an executable
    [ceph-sh-node2][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node2.asok mon_status
    [ceph_deploy.mon][INFO  ] mon.ceph-sh-node2 monitor has reached quorum!
    [ceph_deploy.mon][INFO  ] processing monitor mon.ceph-sh-node3
    [ceph-sh-node3][DEBUG ] connection detected need for sudo
    [ceph-sh-node3][DEBUG ] connected to host: ceph-sh-node3 
    [ceph-sh-node3][DEBUG ] detect platform information from remote host
    [ceph-sh-node3][DEBUG ] detect machine type
    [ceph-sh-node3][DEBUG ] find the location of an executable
    [ceph-sh-node3][INFO  ] Running command: sudo ceph --cluster=ceph --admin-daemon /var/run/ceph/ceph-mon.ceph-sh-node3.asok mon_status
    [ceph_deploy.mon][INFO  ] mon.ceph-sh-node3 monitor has reached quorum!
    [ceph_deploy.mon][INFO  ] all initial monitors are running and have formed quorum
    [ceph_deploy.mon][INFO  ] Running gatherkeys...
    [ceph_deploy.gatherkeys][INFO  ] Storing keys in temp directory /tmp/tmptSPgFH
    [ceph-sh-node1][DEBUG ] connection detected need for sudo
    [ceph-sh-node1][DEBUG ] connected to host: ceph-sh-node1 
    [ceph-sh-node1][DEBUG ] detect platform information from remote host
    [ceph-sh-node1][DEBUG ] detect machine type
    [ceph-sh-node1][DEBUG ] get remote short hostname
    [ceph-sh-node1][DEBUG ] fetch remote file
    [ceph-sh-node1][INFO  ] Running command: sudo /usr/bin/ceph --connect-timeout=25 --cluster=ceph --admin-daemon=/var/run/ceph/ceph-mon.ceph-sh-node1.asok mon_status
    [ceph-sh-node1][INFO  ] Running command: sudo /usr/bin/ceph --connect-timeout=25 --cluster=ceph --name mon. --keyring=/var/lib/ceph/mon/ceph-ceph-sh-node1/keyring auth get client.admin
    [ceph-sh-node1][INFO  ] Running command: sudo /usr/bin/ceph --connect-timeout=25 --cluster=ceph --name mon. --keyring=/var/lib/ceph/mon/ceph-ceph-sh-node1/keyring auth get client.bootstrap-mds
    [ceph-sh-node1][INFO  ] Running command: sudo /usr/bin/ceph --connect-timeout=25 --cluster=ceph --name mon. --keyring=/var/lib/ceph/mon/ceph-ceph-sh-node1/keyring auth get client.bootstrap-mgr
    [ceph-sh-node1][INFO  ] Running command: sudo /usr/bin/ceph --connect-timeout=25 --cluster=ceph --name mon. --keyring=/var/lib/ceph/mon/ceph-ceph-sh-node1/keyring auth get client.bootstrap-osd
    [ceph-sh-node1][INFO  ] Running command: sudo /usr/bin/ceph --connect-timeout=25 --cluster=ceph --name mon. --keyring=/var/lib/ceph/mon/ceph-ceph-sh-node1/keyring auth get client.bootstrap-rgw
    [ceph_deploy.gatherkeys][INFO  ] Storing ceph.client.admin.keyring
    [ceph_deploy.gatherkeys][INFO  ] Storing ceph.bootstrap-mds.keyring
    [ceph_deploy.gatherkeys][INFO  ] Storing ceph.bootstrap-mgr.keyring
    [ceph_deploy.gatherkeys][INFO  ] keyring 'ceph.mon.keyring' already exists
    [ceph_deploy.gatherkeys][INFO  ] Storing ceph.bootstrap-osd.keyring
    [ceph_deploy.gatherkeys][INFO  ] Storing ceph.bootstrap-rgw.keyring
    [ceph_deploy.gatherkeys][INFO  ] Destroy temp directory /tmp/tmptSPgFH
    [ceph-sh-admin@ceph-sh-admin ceph-deploy]$ ll
    total 476
    -rw------- 1 ceph-sh-admin ceph-sh-admin    113 Feb 13 22:06 ceph.bootstrap-mds.keyring
    -rw------- 1 ceph-sh-admin ceph-sh-admin    113 Feb 13 22:06 ceph.bootstrap-mgr.keyring
    -rw------- 1 ceph-sh-admin ceph-sh-admin    113 Feb 13 22:06 ceph.bootstrap-osd.keyring
    -rw------- 1 ceph-sh-admin ceph-sh-admin    113 Feb 13 22:06 ceph.bootstrap-rgw.keyring
    -rw------- 1 ceph-sh-admin ceph-sh-admin    151 Feb 13 22:06 ceph.client.admin.keyring
    -rw-rw-r-- 1 ceph-sh-admin ceph-sh-admin    474 Feb 13 22:00 ceph.conf
    -rw-rw-r-- 1 ceph-sh-admin ceph-sh-admin 270494 Feb 13 22:06 ceph-deploy-ceph.log
    -rw------- 1 ceph-sh-admin ceph-sh-admin     73 Feb 13 21:51 ceph.mon.keyring
    ```
2. mgr

    ```
    [ceph-sh-admin@ceph-sh-admin ceph-deploy]$ ceph-deploy mgr create ceph-sh-node1
    [ceph_deploy.conf][DEBUG ] found configuration file at: /home/ceph-sh-admin/.cephdeploy.conf
    [ceph_deploy.cli][INFO  ] Invoked (2.0.1): /usr/bin/ceph-deploy mgr create ceph-sh-node1
    [ceph_deploy.cli][INFO  ] ceph-deploy options:
    [ceph_deploy.cli][INFO  ]  username                      : None
    [ceph_deploy.cli][INFO  ]  verbose                       : False
    [ceph_deploy.cli][INFO  ]  mgr                           : [('ceph-sh-node1', 'ceph-sh-node1')]
    [ceph_deploy.cli][INFO  ]  overwrite_conf                : False
    [ceph_deploy.cli][INFO  ]  subcommand                    : create
    [ceph_deploy.cli][INFO  ]  quiet                         : False
    [ceph_deploy.cli][INFO  ]  cd_conf                       : <ceph_deploy.conf.cephdeploy.Conf instance at 0x7fae2f5c6ea8>
    [ceph_deploy.cli][INFO  ]  cluster                       : ceph
    [ceph_deploy.cli][INFO  ]  func                          : <function mgr at 0x7fae2fa1b0c8>
    [ceph_deploy.cli][INFO  ]  ceph_conf                     : None
    [ceph_deploy.cli][INFO  ]  default_release               : False
    [ceph_deploy.mgr][DEBUG ] Deploying mgr, cluster ceph hosts ceph-sh-node1:ceph-sh-node1
    [ceph-sh-node1][DEBUG ] connection detected need for sudo
    [ceph-sh-node1][DEBUG ] connected to host: ceph-sh-node1 
    [ceph-sh-node1][DEBUG ] detect platform information from remote host
    [ceph-sh-node1][DEBUG ] detect machine type
    [ceph_deploy.mgr][INFO  ] Distro info: CentOS Linux 7.5.1804 Core
    [ceph_deploy.mgr][DEBUG ] remote host will use systemd
    [ceph_deploy.mgr][DEBUG ] deploying mgr bootstrap to ceph-sh-node1
    [ceph-sh-node1][DEBUG ] write cluster configuration to /etc/ceph/{cluster}.conf
    [ceph-sh-node1][WARNIN] mgr keyring does not exist yet, creating one
    [ceph-sh-node1][DEBUG ] create a keyring file
    [ceph-sh-node1][DEBUG ] create path recursively if it doesn't exist
    [ceph-sh-node1][INFO  ] Running command: sudo ceph --cluster ceph --name client.bootstrap-mgr --keyring /var/lib/ceph/bootstrap-mgr/ceph.keyring auth get-or-create mgr.ceph-sh-node1 mon allow profile mgr osd allow * mds allow * -o /var/lib/ceph/mgr/ceph-ceph-sh-node1/keyring
    [ceph-sh-node1][INFO  ] Running command: sudo systemctl enable ceph-mgr@ceph-sh-node1
    [ceph-sh-node1][WARNIN] Created symlink from /etc/systemd/system/ceph-mgr.target.wants/ceph-mgr@ceph-sh-node1.service to /usr/lib/systemd/system/ceph-mgr@.service.
    [ceph-sh-node1][INFO  ] Running command: sudo systemctl start ceph-mgr@ceph-sh-node1
    [ceph-sh-node1][INFO  ] Running command: sudo systemctl enable ceph.target
    ```
3. osd

    ```
    [ceph-sh-admin@ceph-sh-admin ceph-deploy]$ ceph-deploy osd create --data /dev/sdb ceph-sh-node1
    [ceph_deploy.conf][DEBUG ] found configuration file at: /home/ceph-sh-admin/.cephdeploy.conf
    [ceph_deploy.cli][INFO  ] Invoked (2.0.1): /usr/bin/ceph-deploy osd create --data /dev/sdb ceph-sh-node1
    [ceph_deploy.cli][INFO  ] ceph-deploy options:
    [ceph_deploy.cli][INFO  ]  verbose                       : False
    [ceph_deploy.cli][INFO  ]  bluestore                     : None
    [ceph_deploy.cli][INFO  ]  cd_conf                       : <ceph_deploy.conf.cephdeploy.Conf instance at 0x7fca63f71d88>
    [ceph_deploy.cli][INFO  ]  cluster                       : ceph
    [ceph_deploy.cli][INFO  ]  fs_type                       : xfs
    [ceph_deploy.cli][INFO  ]  block_wal                     : None
    [ceph_deploy.cli][INFO  ]  default_release               : False
    [ceph_deploy.cli][INFO  ]  username                      : None
    [ceph_deploy.cli][INFO  ]  journal                       : None
    [ceph_deploy.cli][INFO  ]  subcommand                    : create
    [ceph_deploy.cli][INFO  ]  host                          : ceph-sh-node1
    [ceph_deploy.cli][INFO  ]  filestore                     : None
    [ceph_deploy.cli][INFO  ]  func                          : <function osd at 0x7fca643c6848>
    [ceph_deploy.cli][INFO  ]  ceph_conf                     : None
    [ceph_deploy.cli][INFO  ]  zap_disk                      : False
    [ceph_deploy.cli][INFO  ]  data                          : /dev/sdb
    [ceph_deploy.cli][INFO  ]  block_db                      : None
    [ceph_deploy.cli][INFO  ]  dmcrypt                       : False
    [ceph_deploy.cli][INFO  ]  overwrite_conf                : False
    [ceph_deploy.cli][INFO  ]  dmcrypt_key_dir               : /etc/ceph/dmcrypt-keys
    [ceph_deploy.cli][INFO  ]  quiet                         : False
    [ceph_deploy.cli][INFO  ]  debug                         : False
    [ceph_deploy.osd][DEBUG ] Creating OSD on cluster ceph with data device /dev/sdb
    [ceph-sh-node1][DEBUG ] connection detected need for sudo
    [ceph-sh-node1][DEBUG ] connected to host: ceph-sh-node1 
    [ceph-sh-node1][DEBUG ] detect platform information from remote host
    [ceph-sh-node1][DEBUG ] detect machine type
    [ceph-sh-node1][DEBUG ] find the location of an executable
    [ceph_deploy.osd][INFO  ] Distro info: CentOS Linux 7.5.1804 Core
    [ceph_deploy.osd][DEBUG ] Deploying osd to ceph-sh-node1
    [ceph-sh-node1][DEBUG ] write cluster configuration to /etc/ceph/{cluster}.conf
    [ceph-sh-node1][WARNIN] osd keyring does not exist yet, creating one
    [ceph-sh-node1][DEBUG ] create a keyring file
    [ceph-sh-node1][DEBUG ] find the location of an executable
    [ceph-sh-node1][INFO  ] Running command: sudo /usr/sbin/ceph-volume --cluster ceph lvm create --bluestore --data /dev/sdb
    [ceph-sh-node1][DEBUG ] Running command: /bin/ceph-authtool --gen-print-key
    [ceph-sh-node1][DEBUG ] Running command: /bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring -i - osd new 0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb
    [ceph-sh-node1][DEBUG ] Running command: /usr/sbin/vgcreate --force --yes ceph-edf80fef-6546-4e23-8949-4d1417459a73 /dev/sdb
    [ceph-sh-node1][DEBUG ]  stdout: Physical volume "/dev/sdb" successfully created.
    [ceph-sh-node1][DEBUG ]  stdout: Volume group "ceph-edf80fef-6546-4e23-8949-4d1417459a73" successfully created
    [ceph-sh-node1][DEBUG ] Running command: /usr/sbin/lvcreate --yes -l 100%FREE -n osd-block-0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb ceph-edf80fef-6546-4e23-8949-4d1417459a73
    [ceph-sh-node1][DEBUG ]  stdout: Logical volume "osd-block-0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb" created.
    [ceph-sh-node1][DEBUG ] Running command: /bin/ceph-authtool --gen-print-key
    [ceph-sh-node1][DEBUG ] Running command: /bin/mount -t tmpfs tmpfs /var/lib/ceph/osd/ceph-0
    [ceph-sh-node1][DEBUG ] Running command: /usr/sbin/restorecon /var/lib/ceph/osd/ceph-0
    [ceph-sh-node1][DEBUG ] Running command: /bin/chown -h ceph:ceph /dev/ceph-edf80fef-6546-4e23-8949-4d1417459a73/osd-block-0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb
    [ceph-sh-node1][DEBUG ] Running command: /bin/chown -R ceph:ceph /dev/dm-2
    [ceph-sh-node1][DEBUG ] Running command: /bin/ln -s /dev/ceph-edf80fef-6546-4e23-8949-4d1417459a73/osd-block-0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb /var/lib/ceph/osd/ceph-0/block
    [ceph-sh-node1][DEBUG ] Running command: /bin/ceph --cluster ceph --name client.bootstrap-osd --keyring /var/lib/ceph/bootstrap-osd/ceph.keyring mon getmap -o /var/lib/ceph/osd/ceph-0/activate.monmap
    [ceph-sh-node1][DEBUG ]  stderr: got monmap epoch 1
    [ceph-sh-node1][DEBUG ] Running command: /bin/ceph-authtool /var/lib/ceph/osd/ceph-0/keyring --create-keyring --name osd.0 --add-key AQA3MWRcx0MpJBAATF1r6j6nZTYl/zsXDlUuTg==
    [ceph-sh-node1][DEBUG ]  stdout: creating /var/lib/ceph/osd/ceph-0/keyring
    [ceph-sh-node1][DEBUG ] added entity osd.0 auth auth(auid = 18446744073709551615 key=AQA3MWRcx0MpJBAATF1r6j6nZTYl/zsXDlUuTg== with 0 caps)
    [ceph-sh-node1][DEBUG ] Running command: /bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0/keyring
    [ceph-sh-node1][DEBUG ] Running command: /bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0/
    [ceph-sh-node1][DEBUG ] Running command: /bin/ceph-osd --cluster ceph --osd-objectstore bluestore --mkfs -i 0 --monmap /var/lib/ceph/osd/ceph-0/activate.monmap --keyfile - --osd-data /var/lib/ceph/osd/ceph-0/ --osd-uuid 0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb --setuser ceph --setgroup ceph
    [ceph-sh-node1][DEBUG ] --> ceph-volume lvm prepare successful for: /dev/sdb
    [ceph-sh-node1][DEBUG ] Running command: /bin/ceph-bluestore-tool --cluster=ceph prime-osd-dir --dev /dev/ceph-edf80fef-6546-4e23-8949-4d1417459a73/osd-block-0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb --path /var/lib/ceph/osd/ceph-0 --no-mon-config
    [ceph-sh-node1][DEBUG ] Running command: /bin/ln -snf /dev/ceph-edf80fef-6546-4e23-8949-4d1417459a73/osd-block-0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb /var/lib/ceph/osd/ceph-0/block
    [ceph-sh-node1][DEBUG ] Running command: /bin/chown -h ceph:ceph /var/lib/ceph/osd/ceph-0/block
    [ceph-sh-node1][DEBUG ] Running command: /bin/chown -R ceph:ceph /dev/dm-2
    [ceph-sh-node1][DEBUG ] Running command: /bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0
    [ceph-sh-node1][DEBUG ] Running command: /bin/systemctl enable ceph-volume@lvm-0-0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb
    [ceph-sh-node1][DEBUG ]  stderr: Created symlink from /etc/systemd/system/multi-user.target.wants/ceph-volume@lvm-0-0bfacce8-06f0-4fcd-8aa7-8c81c117d8fb.service to /usr/lib/systemd/system/ceph-volume@.service.
    [ceph-sh-node1][DEBUG ] Running command: /bin/systemctl enable --runtime ceph-osd@0
    [ceph-sh-node1][DEBUG ]  stderr: Created symlink from /run/systemd/system/ceph-osd.target.wants/ceph-osd@0.service to /usr/lib/systemd/system/ceph-osd@.service.
    [ceph-sh-node1][DEBUG ] Running command: /bin/systemctl start ceph-osd@0
    [ceph-sh-node1][DEBUG ] --> ceph-volume lvm activate successful for osd ID: 0
    [ceph-sh-node1][DEBUG ] --> ceph-volume lvm create successful for: /dev/sdb
    [ceph-sh-node1][INFO  ] checking OSD status...
    [ceph-sh-node1][DEBUG ] find the location of an executable
    [ceph-sh-node1][INFO  ] Running command: sudo /bin/ceph --cluster=ceph osd stat --format=json
    [ceph_deploy.osd][DEBUG ] Host ceph-sh-node1 is now ready for osd use.
    ```