+++
title = "ceph-mgr-dashboard"
description = ""
weight = 4
+++

[dashboard0]: http://docs.ceph.com/docs/master/mgr/dashboard/

# ceph mgr dashboard
**********
## 资源
**********
1. ceph mgr dashboard
[http://docs.ceph.com/docs/master/mgr/dashboard/][dashboard0]  

## 步骤
**********
0. overview

    ```
    1. dashboard
    ```
1. dashboard

    ```
    sudo ceph mgr module enable dashboard
    sudo ceph dashboard create-self-signed-cert
    openssl req -new -nodes -x509 -subj "/O=IT/CN=ceph-mgr-dashboard" -days 3650 -keyout dashboard.key -out dashboard.crt -extensions v3_ca
    sudo ceph config-key set mgr/dashboard/crt -i dashboard.crt
    sudo ceph config-key set mgr/dashboard/key -i dashboard.key
    sudo ceph mgr module disable dashboard
    sudo ceph mgr module enable dashboard
    sudo ceph mgr services
    sudo ceph config set mgr mgr/dashboard/ceph-sh-node1/server_addr 192.168.59.201
    sudo ceph config set mgr mgr/dashboard/ceph-sh-node1/server_port 8443
    sudo ceph config set mgr mgr/dashboard/ceph-sh-node2/server_addr 192.168.59.202
    sudo ceph config set mgr mgr/dashboard/ceph-sh-node2/server_port 8443
    sudo ceph config set mgr mgr/dashboard/ceph-sh-node3/server_addr 192.168.59.203
    sudo ceph config set mgr mgr/dashboard/ceph-sh-node3/server_port 8443
    sudo ceph mgr module disable dashboard
    sudo ceph mgr module enable dashboard
    sudo ceph dashboard set-login-credentials admin admin
    sudo radosgw-admin user create --uid=admin --display-name="admin"
    sudo radosgw-admin caps add --uid=admin --caps="buckets=*;users=*;usage=*;metadata=*"
    sudo radosgw-admin user info --uid=admin
    sudo ceph dashboard set-rgw-api-access-key 4NYA9UOQQ7IDQQE1H7PY
    sudo ceph dashboard set-rgw-api-secret-key t8nBvZCqIS8XRgubecJ2VFCDjA3Fib75Socjdz6z

    ```