+++
title = "ceph-build"
description = ""
weight = 1
+++

[build1]: http://docs.ceph.com/docs/master/install/build-ceph/
[build2]: https://www.cnblogs.com/sisimi/p/7920675.html
[build3]: https://www.waitig.com/ceph%E5%9B%BA%E5%8C%96%E5%AE%9E%E9%AA%8C%EF%BC%88%E4%B8%80%EF%BC%89.html
[build4]: http://www.quts.me/ceph-rpm/
[build5]: https://www.lenzhao.com/topic/59643f73fe30978105e09053
[build6]: http://bean-li.github.io/build-ceph-rpm/
[build7]: https://gist.github.com/wido/0f812dd1dc345cfbd5c38afb0b0dbb4b
[build8]: https://github.com/ceph/autobuild-ceph/blob/master/build-ceph-rpm.sh
[build9]: https://xiexianbin.cn/ceph/2015/12/04/ceph-rpmbuild
[build10]: https://ypdai.github.io/2018/09/21/%E6%A0%B9%E6%8D%AEceph%E6%BA%90%E7%A0%81%E5%88%B6%E4%BD%9CRPM%E5%8C%85/

[install1]: http://docs.ceph.com/ceph-deploy/docs/install.html
[install2]: https://www.lenzhao.com/topic/59643f73fe30978105e09053
[install3]: http://www.zphj1987.com/2016/07/14/%E9%80%9A%E8%BF%87ceph-deploy%E5%AE%89%E8%A3%85%E4%B8%8D%E5%90%8C%E7%89%88%E6%9C%ACceph/
[install4]: http://www.cnxdug.org/?p=2846
[install5]: https://blog.csdn.net/TZ_GG/article/details/80401886
[install6]: https://blog.csdn.net/chuan_day/article/details/60577049
[install7]: https://www.cnblogs.com/linprogram/p/5482513.html
[install8]: http://ceph.org.cn/2016/09/02/%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8%E5%9B%BD%E5%86%85%E6%BA%90%E9%83%A8%E7%BD%B2ceph%EF%BC%9F/

[c++error1]: https://zhoubofsy.github.io/2018/04/02/storage/ceph/compile-luminous/
[c++error2]: https://stackoverflow.com/questions/30887143/make-j-8-g-internal-compiler-error-killed-program-cc1plus
[c++error3]: https://blog.csdn.net/niuzhihuan/article/details/50522138
[c++error4]: https://plumbr.io/outofmemoryerror/kill-process-or-sacrifice-child
[c++error5]: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/performance_tuning_guide/s-memory-captun
[c++error6]: https://blog.csdn.net/renfufei/article/details/78178757
[c++error7]: https://linux.cn/article-5824-1.html
[c++error8]: http://blog.51cto.com/petermis/1119530?tdsourcetag=s_pcqq_aiomsg

[ImportError1]: https://my.oschina.net/u/2460844/blog/532755?p=2
[ImportError2]: https://my.oschina.net/u/2279661/blog/540052
[ImportError3]: https://my.oschina.net/u/2326998/blog/804442
[ImportError4]: http://my.525.life/article?id=1510739742326

# ceph源码编译
**********
## 资源
**********
1. ceph源码编译  
[http://docs.ceph.com/docs/master/install/build-ceph/][build1]  
[源码编译安装ceph][build2]  
[ceph固化实验（一）][build3]  
[Ceph源码构建并安装][build4]  
[ceph源码编译、打包并建立ceph的本地rpm源][build5]  
[build ceph RPM from source code][build6]  
[github wido/ceph-build-rpm.sh][build7]  
[github autobuild-ceph/build-ceph-rpm.sh][build8]  
[Ceph 源码构建与安装][build9]  
[根据ceph源码制作ceph RPM包][build10]  
2. ceph install  
[http://docs.ceph.com/ceph-deploy/docs/install.html][install1]  
[ceph源码编译、打包并建立ceph的本地rpm源][install2]  
[通过ceph-deploy安装不同版本ceph][install3]  
[离线安装ceph的一种方法][install4]  
[搭建本地Ceph yum源][install5]  
[CEPH本地yum源安装][install6]  
[CentOS6.5 本地源搭建Ceph][install7]  
[如何使用国内源部署Ceph？][install8]  
3. ceph -v ImportError  
[ceph的数据存储之路(4)------rbd client 端的数据请求处理][ImportError1]  
[ceph安装部署中，出现ImportError: No module named ceph_*?][ImportError2]  
[ceph编译（二）][ImportError3]  
[使用开源管理控制台监控Ceph][ImportError4]
4. rpmbuild -ba c++: internal compiler error  
[Ceph luminous 编译][c++error1]  
[stackoverflow c++: internal compiler error][c++error2]  
[linux增加虚拟内存][c++error3]  
[Kill process or sacrifice child][c++error4]  
[redhat Out-of-Memory Kill Tunables][c++error5]  
[OutOfMemoryError系列（8）: Kill process or sacrifice child][c++error6]  
[Linux 的 OOM 终结者][c++error7]  
[linux out of memory分析][c++error8]  

## 步骤
**********
0. overview

    ```
    1. clone source
    2. build prerequisites
    3. build ceph
    4. build ceph packages
    5. install with RPM
    6. installing a build
    ```
1. clone source

    ```
    cd /usr/local/src/ceph-mimic
    git clone https://github.com/ceph/ceph.git
    cd ceph
    git branch 13.2.2 v13.2.2
    git checkout 13.2.2
    git submodule update --force --init --recursive

    error: The following untracked working tree files would be overwritten by checkout:
    src/dmclock/.gitignore
    Please move or remove them before you switch branches.
    Aborting
    ```
2. build prerequisites

    ```
    ./install-deps.sh
    ```
3. build ceph

    ```
    ./do_cmake.sh
    cd build
    make
    ```
4. build ceph packages

    ```
    yum install rpm-build rpmdevtools
    rpmdev-setuptree
    ./make-dist 13.2.2
    cp -a boost_1_67_0.tar.bz2 ~/rpmbuild/SOURCES/
    cp -a ceph-13.2.2.tar.bz2 ~/rpmbuild/SOURCES/
    cp -a dashboard_frontend.tar ~/rpmbuild/SOURCES/
    cp -a ceph.spec ~/rpmbuild/SPECS/
    rpmbuild -ba ~/rpmbuild/SPECS/ceph.spec
    ```
    ```
    Checking for unpackaged file(s): /usr/lib/rpm/check-files /root/13.2.4/rpmbuild/BUILDROOT/ceph-13.2.4-0.el7.centos.x86_64
    Wrote: /root/13.2.4/rpmbuild/SRPMS/ceph-13.2.4-0.el7.centos.src.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-base-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-common-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-mds-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-mon-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-mgr-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-fuse-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/rbd-fuse-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/rbd-mirror-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/rbd-nbd-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-radosgw-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-resource-agents-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-osd-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/librados2-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/librados-devel-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/librgw2-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/librgw-devel-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python-rgw-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python34-rgw-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python-rados-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python34-rados-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/libradosstriper1-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/libradosstriper-devel-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/librbd1-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/librbd-devel-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python-rbd-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python34-rbd-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/libcephfs2-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/libcephfs-devel-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python-cephfs-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python34-cephfs-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python34-ceph-argparse-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-test-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/libcephfs_jni1-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/libcephfs_jni-devel-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/cephfs-java-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/rados-objclass-devel-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-selinux-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/python-ceph-compat-13.2.4-0.el7.centos.x86_64.rpm
    Wrote: /root/13.2.4/rpmbuild/RPMS/x86_64/ceph-debuginfo-13.2.4-0.el7.centos.x86_64.rpm
    Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.4prYE3
    + umask 022
    + cd /root/13.2.4/rpmbuild/BUILD
    + cd ceph-13.2.4
    + rm -rf /root/13.2.4/rpmbuild/BUILDROOT/ceph-13.2.4-0.el7.centos.x86_64
    + exit 0
    ```
    ```
    Checking for unpackaged file(s): /usr/lib/rpm/check-files /root/rpmbuild/BUILDROOT/ceph-13.2.2-0.el7.centos.x86_64
    Wrote: /root/rpmbuild/SRPMS/ceph-13.2.2-0.el7.centos.src.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-base-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-common-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-mds-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-mon-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-mgr-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-fuse-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/rbd-fuse-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/rbd-mirror-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/rbd-nbd-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-radosgw-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-resource-agents-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-osd-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/librados2-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/librados-devel-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/librgw2-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/librgw-devel-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python-rgw-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python34-rgw-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python-rados-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python34-rados-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/libradosstriper1-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/libradosstriper-devel-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/librbd1-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/librbd-devel-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python-rbd-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python34-rbd-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/libcephfs2-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/libcephfs-devel-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python-cephfs-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python34-cephfs-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python34-ceph-argparse-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-test-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/libcephfs_jni1-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/libcephfs_jni-devel-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/cephfs-java-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/rados-objclass-devel-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-selinux-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/python-ceph-compat-13.2.2-0.el7.centos.x86_64.rpm
    Wrote: /root/rpmbuild/RPMS/x86_64/ceph-debuginfo-13.2.2-0.el7.centos.x86_64.rpm
    Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.eyBYYL
    + umask 022
    + cd /root/rpmbuild/BUILD
    + cd ceph-13.2.2
    + rm -rf /root/rpmbuild/BUILDROOT/ceph-13.2.2-0.el7.centos.x86_64
    + exit 0
    ```
    ```
    [root@ceph-sh-admin ~]# rpmbuild -ba ~/rpmbuild/SPECS/ceph.spec 
    error: Failed build dependencies:
        devtoolset-7-gcc-c++ is needed by ceph-2:13.2.2-0.el7.centos.x86_64
        liboath-devel is needed by ceph-2:13.2.2-0.el7.centos.x86_64
        CUnit-devel is needed by ceph-2:13.2.2-0.el7.centos.x86_64
        lz4-devel >= 1.7 is needed by ceph-2:13.2.2-0.el7.centos.x86_64
    ```
    ```
    [root@ceph-sh-admin ceph]# ls
    admin                     ceph.spec.in      COPYING-GPL2      do_freebsd.sh    keys            man                  README.alpine.md    run-make-check.sh      udev
    alpine                    cmake             COPYING-LGPL2.1   Doxyfile         librarytest.sh  mirroring            README.FreeBSD      selinux
    AUTHORS                   CMakeLists.txt    debian            etc              make-apk.sh     PendingReleaseNotes  README.git-subtree  share
    bin                       CodingStyle       doc               examples         make-debs.sh    pom.xml              README.md           src
    ceph-erasure-code-corpus  CONTRIBUTING.rst  doc_deps.deb.txt  fusetrace        make-dist       qa                   README.solaris      SubmittingPatches.rst
    ceph-object-corpus        COPYING           do_cmake.sh       install-deps.sh  make-srpm.sh    README.aix           README.xio          systemd
    ```
    ```
    [root@ceph-sh-admin ceph]# ls
    admin                 ceph-erasure-code-corpus  CodingStyle             debian            etc              make-apk.sh   PendingReleaseNotes  README.git-subtree  share
    alpine                ceph-object-corpus        CONTRIBUTING.rst        doc               examples         make-debs.sh  pom.xml              README.md           src
    AUTHORS               ceph.spec                 COPYING                 doc_deps.deb.txt  fusetrace        make-dist     qa                   README.solaris      SubmittingPatches.rst
    bin                   ceph.spec.in              COPYING-GPL2            do_cmake.sh       install-deps.sh  make-srpm.sh  README.aix           README.xio          systemd
    boost_1_67_0.tar.bz2  cmake                     COPYING-LGPL2.1         do_freebsd.sh     keys             man           README.alpine.md     run-make-check.sh   udev
    ceph-13.2.2.tar.bz2   CMakeLists.txt            dashboard_frontend.tar  Doxyfile          librarytest.sh   mirroring     README.FreeBSD       selinux
    ```
    ```
    [root@ceph-sh-admin ceph]# ./make-dist 13.2.2
    version 13.2.2
    updating submodules...
    Synchronizing submodule url for 'ceph-erasure-code-corpus'
    Synchronizing submodule url for 'ceph-object-corpus'
    Synchronizing submodule url for 'src/blkin'
    Synchronizing submodule url for 'src/civetweb'
    Synchronizing submodule url for 'src/crypto/isa-l/isa-l_crypto'
    Synchronizing submodule url for 'src/erasure-code/jerasure/gf-complete'
    Synchronizing submodule url for 'src/erasure-code/jerasure/jerasure'
    Synchronizing submodule url for 'src/googletest'
    Synchronizing submodule url for 'src/isa-l'
    Synchronizing submodule url for 'src/lua'
    Synchronizing submodule url for 'src/rapidjson'
    Synchronizing submodule url for 'src/rocksdb'
    Synchronizing submodule url for 'src/spdk'
    Synchronizing submodule url for 'src/xxHash'
    Synchronizing submodule url for 'src/zstd'
    Submodule path 'ceph-erasure-code-corpus': checked out '2d7d78b9cc52e8a9529d8cc2d2954c7d375d5dd7'
    Submodule path 'ceph-object-corpus': checked out 'e32bf8ca3dc6151ebe7f205ba187815bc18e1cef'
    Submodule path 'src/blkin': checked out 'f24ceec055ea236a093988237a9821d145f5f7c8'
    Submodule path 'src/civetweb': checked out 'ff2881e2cd5869a71ca91083bad5d12cccd22136'
    Submodule path 'src/crypto/isa-l/isa-l_crypto': checked out '603529a4e06ac8a1662c13d6b31f122e21830352'
    Submodule path 'src/erasure-code/jerasure/gf-complete': checked out '7e61b44404f0ed410c83cfd3947a52e88ae044e1'
    Submodule path 'src/erasure-code/jerasure/jerasure': checked out '96c76b89d661c163f65a014b8042c9354ccf7f31'
    Submodule path 'src/googletest': checked out 'fdb850479284e2aae047b87df6beae84236d0135'
    Submodule path 'src/isa-l': checked out '7e1a337433a340bc0974ed0f04301bdaca374af6'
    Submodule path 'src/lua': checked out '1fce39c6397056db645718b8f5821571d97869a4'
    Submodule path 'src/rapidjson': checked out 'f54b0e47a08782a6131cc3d60f94d038fa6e0a51'
    Submodule path 'src/rapidjson/thirdparty/gtest': checked out '0a439623f75c029912728d80cb7f1b8b48739ca4'
    Submodule path 'src/rocksdb': checked out 'f4a857da0b720691effc524469f6db895ad00d8e'
    Submodule path 'src/spdk': checked out 'f474ce6930f0a44360e1cc4ecd606d2348481c4c'
    Submodule path 'src/spdk/dpdk': checked out '6ece49ad5a26f5e2f5c4af6c06c30376c0ddc387'
    Submodule path 'src/xxHash': checked out '1f40c6511fa8dd9d2e337ca8c9bc18b3e87663c9'
    Submodule path 'src/zstd': checked out 'f4340f46b2387bc8de7d5320c0b83bb1499933ad'
    cleanup...
    building tarball...
    creating superproject archive...done
    looking for subprojects...done
    found:
        src/blkin/
        src/civetweb/
        src/crypto/isa-l/isa-l_crypto/
        src/erasure-code/jerasure/gf-complete/
        src/erasure-code/jerasure/jerasure/
        src/googletest/
        src/isa-l/
        src/lua/
        src/rapidjson/
        src/rapidjson/thirdparty/gtest/
        src/rocksdb/
        src/spdk/
        src/spdk/dpdk/
        src/xxHash/
        src/zstd/
    archiving submodules...done
    concatenating archives into single archive...done
    moving archive to ceph-13.2.2.tar...done
    including src/.git_version, ceph.spec
    ceph-13.2.2/src/.git_version
    ceph-13.2.2/ceph.spec
    ceph-13.2.2/alpine/APKBUILD
    2018-12-19 22:39:13 URL:https://d29vzk4ow07wi7.cloudfront.net/2684c972994ee57fc5632e03bf044746f6eb45d4920c343937a465fd67a5adba?response-content-disposition=attachment%3Bfilename%3D%22boost_1_67_0.tar.bz2%22&Policy=eyJTdGF0ZW1lbnQiOiBbeyJSZXNvdXJjZSI6Imh0dHAqOi8vZDI5dnprNG93MDd3aTcuY2xvdWRmcm9udC5uZXQvMjY4NGM5NzI5OTRlZTU3ZmM1NjMyZTAzYmYwNDQ3NDZmNmViNDVkNDkyMGMzNDM5MzdhNDY1ZmQ2N2E1YWRiYT9yZXNwb25zZS1jb250ZW50LWRpc3Bvc2l0aW9uPWF0dGFjaG1lbnQlM0JmaWxlbmFtZSUzRCUyMmJvb3N0XzFfNjdfMC50YXIuYnoyJTIyIiwiQ29uZGl0aW9uIjp7IkRhdGVMZXNzVGhhbiI6eyJBV1M6RXBvY2hUaW1lIjoxNTQ1MjMxMDU1fSwiSXBBZGRyZXNzIjp7IkFXUzpTb3VyY2VJcCI6IjAuMC4wLjAvMCJ9fX1dfQ__&Signature=YETLjGQpS-kM4d-hPjK~pwoe6VhRRI-UexIThDkVgb1vkWK2TOBwueVUh1BuRnI~SD92k8AFOVGS2nlUgim20KeB~h2679zDtfCe45luB9c~LttuHxf3nh8t5SB2KjqAfvqZ~WS0DVg~Ze2nb9g1IKqcbUcQIB4mlZ7~sLVXWdjPZyIO97~WjWjYHjm~PHKidSi23qfPrbedTXlP0NiaieM81H95qmKP2eNPEcwLz-Ogqkm5glI9~NsqHifUAsffqNqp6L00UG5sy4pHiEEgIzjBHNY81x12wjjBLE3Pfw69FVmaq09N3ZDB1Y44Xsi4WUSijCP7ZmQkRK0nUsv6qg__&Key-Pair-Id=APKAIFKFWOMXM2UMTSFA [87336566/87336566] -> "boost_1_67_0.tar.bz2" [1]
    Running virtualenv with interpreter /usr/bin/python2.7
    New python executable in /tmp/tmp.fN4lEiece2/bin/python2.7
    Also creating executable in /tmp/tmp.fN4lEiece2/bin/python
    Installing setuptools, pip, wheel...done.
    Collecting pip>=6.1
    Downloading https://files.pythonhosted.org/packages/c2/d7/90f34cb0d83a6c5631cf71dfe64cc1054598c843a92b400e55675cc2ac37/pip-18.1-py2.py3-none-any.whl (1.3MB)
        100% |████████████████████████████████| 1.3MB 793kB/s 
    Installing collected packages: pip
    Found existing installation: pip 9.0.1
        Uninstalling pip-9.0.1:
        Successfully uninstalled pip-9.0.1
    Successfully installed pip-18.1
    Collecting setuptools<36
    Downloading https://files.pythonhosted.org/packages/27/45/79618f80704497f74f2de1ca62216a5c3ffdbd49f43047c81c30e126a055/setuptools-35.0.2-py2.py3-none-any.whl (390kB)
        100% |████████████████████████████████| 399kB 11.3MB/s 
    Collecting appdirs>=1.4.0 (from setuptools<36)
    Downloading https://files.pythonhosted.org/packages/56/eb/810e700ed1349edde4cbdc1b2a21e28cdf115f9faf263f6bbf8447c1abf3/appdirs-1.4.3-py2.py3-none-any.whl
    Collecting packaging>=16.8 (from setuptools<36)
    Downloading https://files.pythonhosted.org/packages/89/d1/92e6df2e503a69df9faab187c684585f0136662c12bb1f36901d426f3fab/packaging-18.0-py2.py3-none-any.whl
    Collecting six>=1.6.0 (from setuptools<36)
    Downloading https://files.pythonhosted.org/packages/73/fb/00a976f728d0d1fecfe898238ce23f502a721c0ac0ecfedb80e0d88c64e9/six-1.12.0-py2.py3-none-any.whl
    Collecting pyparsing>=2.0.2 (from packaging>=16.8->setuptools<36)
    Downloading https://files.pythonhosted.org/packages/71/e8/6777f6624681c8b9701a8a0a5654f3eb56919a01a78e12bf3c73f5a3c714/pyparsing-2.3.0-py2.py3-none-any.whl (59kB)
        100% |████████████████████████████████| 61kB 18.2MB/s 
    Installing collected packages: appdirs, pyparsing, six, packaging, setuptools
    Found existing installation: setuptools 28.8.0
        Uninstalling setuptools-28.8.0:
        Successfully uninstalled setuptools-28.8.0
    Successfully installed appdirs-1.4.3 packaging-18.0 pyparsing-2.3.0 setuptools-35.0.2 six-1.12.0
    Looking in links: file:///usr/local/src/ceph-mimic/ceph/wheelhouse
    Collecting tox>=2.9.1
    Url 'file:///usr/local/src/ceph-mimic/ceph/wheelhouse' is ignored: it is neither a file nor a directory.
    Downloading https://files.pythonhosted.org/packages/30/51/e5c4dda814ffc84204321c5074c767d11d6bdd7d1082a6dc63ed28f0eadf/tox-3.6.0-py2.py3-none-any.whl (53kB)
        100% |████████████████████████████████| 61kB 498kB/s 
    Collecting virtualenv>=1.11.2 (from tox>=2.9.1)
    Url 'file:///usr/local/src/ceph-mimic/ceph/wheelhouse' is ignored: it is neither a file nor a directory.
    Downloading https://files.pythonhosted.org/packages/7c/17/9b7b6cddfd255388b58c61e25b091047f6814183e1d63741c8df8dcd65a2/virtualenv-16.1.0-py2.py3-none-any.whl (1.9MB)
        100% |████████████████████████████████| 1.9MB 3.3MB/s 
    Collecting toml>=0.9.4 (from tox>=2.9.1)
    Url 'file:///usr/local/src/ceph-mimic/ceph/wheelhouse' is ignored: it is neither a file nor a directory.
    Downloading https://files.pythonhosted.org/packages/a2/12/ced7105d2de62fa7c8fb5fce92cc4ce66b57c95fb875e9318dba7f8c5db0/toml-0.10.0-py2.py3-none-any.whl
    Collecting pluggy<1,>=0.3.0 (from tox>=2.9.1)
    Url 'file:///usr/local/src/ceph-mimic/ceph/wheelhouse' is ignored: it is neither a file nor a directory.
    Downloading https://files.pythonhosted.org/packages/1c/e7/017c262070af41fe251401cb0d0e1b7c38f656da634cd0c15604f1f30864/pluggy-0.8.0-py2.py3-none-any.whl
    Requirement already satisfied: six<2,>=1.0.0 in /tmp/tmp.fN4lEiece2/lib/python2.7/site-packages (from tox>=2.9.1) (1.12.0)
    Collecting py<2,>=1.4.17 (from tox>=2.9.1)
    Url 'file:///usr/local/src/ceph-mimic/ceph/wheelhouse' is ignored: it is neither a file nor a directory.
    Downloading https://files.pythonhosted.org/packages/3e/c7/3da685ef117d42ac8d71af525208759742dd235f8094221fdaafcd3dba8f/py-1.7.0-py2.py3-none-any.whl (83kB)
        100% |████████████████████████████████| 92kB 23.3MB/s 
    Requirement already satisfied: setuptools>=30.0.0 in /tmp/tmp.fN4lEiece2/lib/python2.7/site-packages (from tox>=2.9.1) (35.0.2)
    Collecting filelock<4,>=3.0.0 (from tox>=2.9.1)
    Url 'file:///usr/local/src/ceph-mimic/ceph/wheelhouse' is ignored: it is neither a file nor a directory.
    Downloading https://files.pythonhosted.org/packages/2a/bd/6a87635dba4906ae56377b22f64805b2f00d8cafb26e411caaf3559a5475/filelock-3.0.10.tar.gz
    Requirement already satisfied: appdirs>=1.4.0 in /tmp/tmp.fN4lEiece2/lib/python2.7/site-packages (from setuptools>=30.0.0->tox>=2.9.1) (1.4.3)
    Requirement already satisfied: packaging>=16.8 in /tmp/tmp.fN4lEiece2/lib/python2.7/site-packages (from setuptools>=30.0.0->tox>=2.9.1) (18.0)
    Requirement already satisfied: pyparsing>=2.0.2 in /tmp/tmp.fN4lEiece2/lib/python2.7/site-packages (from packaging>=16.8->setuptools>=30.0.0->tox>=2.9.1) (2.3.0)
    Building wheels for collected packages: filelock
    Running setup.py bdist_wheel for filelock ... done
    Stored in directory: /root/.cache/pip/wheels/46/b3/26/8803692ec1f1729fcf201583b5de74f112da1f1488f36e47b0
    Successfully built filelock
    Installing collected packages: virtualenv, toml, pluggy, py, filelock, tox
    Successfully installed filelock-3.0.10 pluggy-0.8.0 py-1.7.0 toml-0.10.0 tox-3.6.0 virtualenv-16.1.0
    Collecting nodeenv
    Downloading https://files.pythonhosted.org/packages/00/6e/ed417bd1ed417ab3feada52d0c89ab0ed87d150f91590badf84273e047c9/nodeenv-1.3.3.tar.gz
    Building wheels for collected packages: nodeenv
    Running setup.py bdist_wheel for nodeenv ... done
    Stored in directory: /root/.cache/pip/wheels/7b/6c/23/eb26369b77904c8963fae9e64338b0f0b948b4d59710760834
    Successfully built nodeenv
    Installing collected packages: nodeenv
    Successfully installed nodeenv-1.3.3
    * Install prebuilt node (8.10.0) ..... done.
    * Appending data to /tmp/tmp.fN4lEiece2/bin/activate
    * Appending data to /tmp/tmp.fN4lEiece2/bin/activate.fish
    npm WARN notice [SECURITY] lodash has the following vulnerability: 1 low. Go here for more details: https://nodesecurity.io/advisories?search=lodash&version=3.10.1 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.
    npm WARN deprecated browserslist@2.11.3: Browserslist 2 could fail on reading Browserslist >3.0 config used in other tools.
    npm WARN notice [SECURITY] debug has the following vulnerability: 1 low. Go here for more details: https://nodesecurity.io/advisories?search=debug&version=2.3.3 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.
    npm WARN notice [SECURITY] adm-zip has the following vulnerability: 1 high. Go here for more details: https://nodesecurity.io/advisories?search=adm-zip&version=0.4.4 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.
    npm WARN notice [SECURITY] debug has the following vulnerability: 1 low. Go here for more details: https://nodesecurity.io/advisories?search=debug&version=2.2.0 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.
    npm WARN notice [SECURITY] ws has the following vulnerability: 1 high. Go here for more details: https://nodesecurity.io/advisories?search=ws&version=1.1.2 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.
    npm WARN notice [SECURITY] hoek has the following vulnerability: 1 moderate. Go here for more details: https://nodesecurity.io/advisories?search=hoek&version=2.16.3 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.
    npm WARN deprecated hoek@2.16.3: This version is no longer maintained. Please upgrade to the latest version.
    npm WARN deprecated boom@2.10.1: This version is no longer maintained. Please upgrade to the latest version.
    npm WARN deprecated cryptiles@2.0.5: This version is no longer maintained. Please upgrade to the latest version.
    npm WARN notice [SECURITY] webpack-dev-server has the following vulnerability: 1 high. Go here for more details: https://nodesecurity.io/advisories?search=webpack-dev-server&version=2.11.3 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.
    npm WARN notice [SECURITY] parsejson has the following vulnerability: 1 high. Go here for more details: https://nodesecurity.io/advisories?search=parsejson&version=0.0.3 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.

    > node-sass@4.11.0 install /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/node-sass
    > node scripts/install.js

    Downloading binary from https://github.com/sass/node-sass/releases/download/v4.11.0/linux-x64-57_binding.node
    Download complete .] - :
    Binary saved to /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/node-sass/vendor/linux-x64-57/binding.node
    Caching binary to /root/.npm/node-sass/4.11.0/linux-x64-57_binding.node

    > phantomjs-prebuilt@2.1.16 install /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/phantomjs-prebuilt
    > node install.js

    PhantomJS not found on PATH
    Downloading https://github.com/Medium/phantomjs/releases/download/v2.1.1/phantomjs-2.1.1-linux-x86_64.tar.bz2
    Saving to /tmp/phantomjs/phantomjs-2.1.1-linux-x86_64.tar.bz2
    Receiving...
    [=======================================-] 97%
    Received 22866K total.
    Extracting tar contents (via spawned process)
    Removing /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/phantomjs-prebuilt/lib/phantom
    Copying extracted folder /tmp/phantomjs/phantomjs-2.1.1-linux-x86_64.tar.bz2-extract-1545230504512/phantomjs-2.1.1-linux-x86_64 -> /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/phantomjs-prebuilt/lib/phantom
    Writing location.js file
    Done. Phantomjs binary available at /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/phantomjs-prebuilt/lib/phantom/bin/phantomjs

    > uglifyjs-webpack-plugin@0.4.6 postinstall /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/webpack/node_modules/uglifyjs-webpack-plugin
    > node lib/post_install.js


    > node-sass@4.11.0 postinstall /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/node-sass
    > node scripts/build.js

    Binary found at /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend/node_modules/node-sass/vendor/linux-x64-57/binding.node
    Testing binary
    Binary is fine
    npm notice created a lockfile as package-lock.json. You should commit this file.
    npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.4 (node_modules/fsevents):
    npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.4: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

    added 1212 packages in 83.122s

    > ceph-dashboard@0.0.0 build /usr/local/src/ceph-mimic/ceph/src/pybind/mgr/dashboard/frontend
    > ng build "--prod"

    Date: 2018-12-19T14:44:35.989Z                                                            
    Hash: 4d54470aa4df27ef5d11
    Time: 160520ms
    chunk {scripts} scripts.fc88ef4a23399c760d0b.bundle.js (scripts) 210 kB [initial] [rendered]
    chunk {0} main.9161e6b9f915df99731f.bundle.js (main) 2.25 MB [initial] [rendered]
    chunk {1} polyfills.e77041c9a5fafeebff7d.bundle.js (polyfills) 101 kB [initial] [rendered]
    chunk {2} styles.4a0354ff413d7450a4e3.bundle.css (styles) 211 kB [initial] [rendered]
    chunk {3} inline.318b50c57b4eba3d437b.bundle.js (inline) 796 bytes [entry] [rendered]

    WARNING in Invalid animation value at 11938:14. Ignoring.

    WARNING in Invalid animation value at 11937:22. Ignoring.
    compressing...
    done.
    ```

5. install with RPM

    ```
    cd /var/www/html/ceph/13.2.4/el7
    sudo cp -R ~/rpm/13.2.4/rpmbuild/RPMS/x86_64 .
    sudo cp -R ~/rpm/13.2.4/rpmbuild/SRPMS .
    sudo yum install -y createrepo httpd
    sudo createrepo -p /var/www/html/ceph/13.2.4/el7/x86_64
    sudo createrepo -p /var/www/html/ceph/13.2.4/el7/SRPMS
    rpm -qi centos-release
    arch
    sudo systemctl enable httpd.service
    sudo systemctl start httpd.service
    sudo systemctl status httpd.service
    vi  /etc/selinux/config
    SELINUX=disabled
    reboot
    ceph.conf
    yum clean all
    yum makecache
    yum list |grep ceph
    rpm -ivh https://download.ceph.com/rpm-mimic/el7/noarch/ceph-deploy-2.0.1-0.noarch.rpm
    ceph-deploy install --no-adjust-repos ceph-sh-node1
    ```
    ```
    [ceph-sh-admin@ceph-sh-admin x86_64]$ pwd
    /var/www/html/ceph/13.2.4/el7/x86_64
    [ceph-sh-admin@ceph-sh-admin x86_64]$ sudo createrepo .
    Spawning worker 0 with 40 pkgs
    Workers Finished
    Saving Primary metadata
    Saving file lists metadata
    Saving other metadata
    Generating sqlite DBs
    Sqlite DBs complete
    ```
    ```
    [ceph]
    name=Ceph packages for $basearch
    baseurl=http://192.168.59.200/ceph/13.2.4/el7/x86_64
    enabled=1
    priority=2
    gpgcheck=0

    [ceph-source]
    name=Ceph source packages
    baseurl=http://192.168.59.200/ceph/13.2.4/el7/SRPMS
    enabled=1
    priority=2
    gpgcheck=0
    ```
    ```
    [ceph-sh-admin@ceph-sh-admin ~]$ cd ceph-deploy/
    [ceph-sh-admin@ceph-sh-admin ceph-deploy]$ ls
    [ceph-sh-admin@ceph-sh-admin ceph-deploy]$ ceph-deploy install --no-adjust-repos ceph-sh-node1
    [ceph_deploy.conf][DEBUG ] found configuration file at: /home/ceph-sh-admin/.cephdeploy.conf
    [ceph_deploy.cli][INFO  ] Invoked (2.0.1): /usr/bin/ceph-deploy install --no-adjust-repos ceph-sh-node1
    [ceph_deploy.cli][INFO  ] ceph-deploy options:
    [ceph_deploy.cli][INFO  ]  verbose                       : False
    [ceph_deploy.cli][INFO  ]  testing                       : None
    [ceph_deploy.cli][INFO  ]  cd_conf                       : <ceph_deploy.conf.cephdeploy.Conf instance at 0x7f13713db0e0>
    [ceph_deploy.cli][INFO  ]  cluster                       : ceph
    [ceph_deploy.cli][INFO  ]  dev_commit                    : None
    [ceph_deploy.cli][INFO  ]  install_mds                   : False
    [ceph_deploy.cli][INFO  ]  stable                        : None
    [ceph_deploy.cli][INFO  ]  default_release               : False
    [ceph_deploy.cli][INFO  ]  username                      : None
    [ceph_deploy.cli][INFO  ]  adjust_repos                  : False
    [ceph_deploy.cli][INFO  ]  func                          : <function install at 0x7f1371817578>
    [ceph_deploy.cli][INFO  ]  install_mgr                   : False
    [ceph_deploy.cli][INFO  ]  install_all                   : False
    [ceph_deploy.cli][INFO  ]  repo                          : False
    [ceph_deploy.cli][INFO  ]  host                          : ['ceph-sh-node1']
    [ceph_deploy.cli][INFO  ]  install_rgw                   : False
    [ceph_deploy.cli][INFO  ]  install_tests                 : False
    [ceph_deploy.cli][INFO  ]  repo_url                      : None
    [ceph_deploy.cli][INFO  ]  ceph_conf                     : None
    [ceph_deploy.cli][INFO  ]  install_osd                   : False
    [ceph_deploy.cli][INFO  ]  version_kind                  : stable
    [ceph_deploy.cli][INFO  ]  install_common                : False
    [ceph_deploy.cli][INFO  ]  overwrite_conf                : False
    [ceph_deploy.cli][INFO  ]  quiet                         : False
    [ceph_deploy.cli][INFO  ]  dev                           : master
    [ceph_deploy.cli][INFO  ]  nogpgcheck                    : False
    [ceph_deploy.cli][INFO  ]  local_mirror                  : None
    [ceph_deploy.cli][INFO  ]  release                       : None
    [ceph_deploy.cli][INFO  ]  install_mon                   : False
    [ceph_deploy.cli][INFO  ]  gpg_url                       : None
    [ceph_deploy.install][DEBUG ] Installing stable version mimic on cluster ceph hosts ceph-sh-node1
    [ceph_deploy.install][DEBUG ] Detecting platform for host ceph-sh-node1 ...
    [ceph-sh-node1][DEBUG ] connection detected need for sudo
    [ceph-sh-node1][DEBUG ] connected to host: ceph-sh-node1 
    [ceph-sh-node1][DEBUG ] detect platform information from remote host
    [ceph-sh-node1][DEBUG ] detect machine type
    [ceph_deploy.install][INFO  ] Distro info: CentOS Linux 7.5.1804 Core
    [ceph-sh-node1][INFO  ] installing Ceph on ceph-sh-node1
    [ceph-sh-node1][INFO  ] Running command: sudo yum clean all
    [ceph-sh-node1][DEBUG ] Loaded plugins: fastestmirror, priorities
    [ceph-sh-node1][DEBUG ] Cleaning repos: base ceph ceph-source cr epel extras updates
    [ceph-sh-node1][DEBUG ] Cleaning up everything
    [ceph-sh-node1][DEBUG ] Maybe you want: rm -rf /var/cache/yum, to also free up space taken by orphaned data from disabled or removed repos
    [ceph-sh-node1][DEBUG ] Cleaning up list of fastest mirrors
    [ceph-sh-node1][INFO  ] Running command: sudo yum -y install ceph ceph-radosgw
    [ceph-sh-node1][DEBUG ] Loaded plugins: fastestmirror, priorities
    [ceph-sh-node1][DEBUG ] Determining fastest mirrors
    [ceph-sh-node1][DEBUG ]  * base: mirrors.163.com
    [ceph-sh-node1][DEBUG ]  * epel: mirror.premi.st
    [ceph-sh-node1][DEBUG ]  * extras: ftp.cuhk.edu.hk
    [ceph-sh-node1][DEBUG ]  * updates: mirrors.aliyun.com
    [ceph-sh-node1][DEBUG ] 7 packages excluded due to repository priority protections
    [ceph-sh-node1][DEBUG ] Resolving Dependencies
    [ceph-sh-node1][DEBUG ] --> Running transaction check
    [ceph-sh-node1][DEBUG ] ---> Package ceph.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: ceph-osd = 2:13.2.4-0.el7.centos for package: 2:ceph-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: ceph-mon = 2:13.2.4-0.el7.centos for package: 2:ceph-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: ceph-mgr = 2:13.2.4-0.el7.centos for package: 2:ceph-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: ceph-mds = 2:13.2.4-0.el7.centos for package: 2:ceph-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] ---> Package ceph-radosgw.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: librgw2 = 2:13.2.4-0.el7.centos for package: 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: librados2 = 2:13.2.4-0.el7.centos for package: 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: ceph-selinux = 2:13.2.4-0.el7.centos for package: 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: ceph-common = 2:13.2.4-0.el7.centos for package: 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: mailcap for package: 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: librados.so.2()(64bit) for package: 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: liboath.so.0()(64bit) for package: 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: libceph-common.so.0()(64bit) for package: 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Running transaction check
    [ceph-sh-node1][DEBUG ] ---> Package ceph-common.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-rgw = 2:13.2.4-0.el7.centos for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-rbd = 2:13.2.4-0.el7.centos for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-rados = 2:13.2.4-0.el7.centos for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-cephfs = 2:13.2.4-0.el7.centos for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: librbd1 = 2:13.2.4-0.el7.centos for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: libradosstriper1 = 2:13.2.4-0.el7.centos for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: libcephfs2 = 2:13.2.4-0.el7.centos for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: librbd.so.1()(64bit) for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: libradosstriper.so.1()(64bit) for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: libcephfs.so.2()(64bit) for package: 2:ceph-common-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] ---> Package ceph-mds.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: ceph-base = 2:13.2.4-0.el7.centos for package: 2:ceph-mds-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] ---> Package ceph-mgr.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-routes for package: 2:ceph-mgr-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-bcrypt for package: 2:ceph-mgr-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] ---> Package ceph-mon.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package ceph-osd.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package ceph-selinux.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package liboath.x86_64 0:2.4.1-9.el7 will be installed
    [ceph-sh-node1][DEBUG ] ---> Package librados2.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package librgw2.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package mailcap.noarch 0:2.1.41-2.el7 will be installed
    [ceph-sh-node1][DEBUG ] --> Running transaction check
    [ceph-sh-node1][DEBUG ] ---> Package ceph-base.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: gdisk for package: 2:ceph-base-13.2.4-0.el7.centos.x86_64
    [ceph-sh-node1][DEBUG ] ---> Package libcephfs2.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package libradosstriper1.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package librbd1.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package python-cephfs.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package python-rados.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package python-rbd.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package python-rgw.x86_64 2:13.2.4-0.el7.centos will be installed
    [ceph-sh-node1][DEBUG ] ---> Package python-routes.noarch 0:1.13-2.el7 will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-repoze-lru for package: python-routes-1.13-2.el7.noarch
    [ceph-sh-node1][DEBUG ] ---> Package python2-bcrypt.x86_64 0:3.1.4-4.el7 will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-cffi for package: python2-bcrypt-3.1.4-4.el7.x86_64
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python2-six for package: python2-bcrypt-3.1.4-4.el7.x86_64
    [ceph-sh-node1][DEBUG ] --> Running transaction check
    [ceph-sh-node1][DEBUG ] ---> Package gdisk.x86_64 0:0.8.10-2.el7 will be installed
    [ceph-sh-node1][DEBUG ] ---> Package python-cffi.x86_64 0:1.6.0-5.el7 will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-pycparser for package: python-cffi-1.6.0-5.el7.x86_64
    [ceph-sh-node1][DEBUG ] ---> Package python-repoze-lru.noarch 0:0.4-3.el7 will be installed
    [ceph-sh-node1][DEBUG ] ---> Package python2-six.noarch 0:1.9.0-0.el7 will be installed
    [ceph-sh-node1][DEBUG ] --> Running transaction check
    [ceph-sh-node1][DEBUG ] ---> Package python-pycparser.noarch 0:2.14-1.el7 will be installed
    [ceph-sh-node1][DEBUG ] --> Processing Dependency: python-ply for package: python-pycparser-2.14-1.el7.noarch
    [ceph-sh-node1][DEBUG ] --> Running transaction check
    [ceph-sh-node1][DEBUG ] ---> Package python-ply.noarch 0:3.4-11.el7 will be installed
    [ceph-sh-node1][DEBUG ] --> Finished Dependency Resolution
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] Dependencies Resolved
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] ================================================================================
    [ceph-sh-node1][DEBUG ]  Package                Arch        Version                     Repository
    [ceph-sh-node1][DEBUG ]                                                                            Size
    [ceph-sh-node1][DEBUG ] ================================================================================
    [ceph-sh-node1][DEBUG ] Installing:
    [ceph-sh-node1][DEBUG ]  ceph                   x86_64      2:13.2.4-0.el7.centos       ceph      1.9 k
    [ceph-sh-node1][DEBUG ]  ceph-radosgw           x86_64      2:13.2.4-0.el7.centos       ceph      4.5 M
    [ceph-sh-node1][DEBUG ] Installing for dependencies:
    [ceph-sh-node1][DEBUG ]  ceph-base              x86_64      2:13.2.4-0.el7.centos       ceph      4.9 M
    [ceph-sh-node1][DEBUG ]  ceph-common            x86_64      2:13.2.4-0.el7.centos       ceph       14 M
    [ceph-sh-node1][DEBUG ]  ceph-mds               x86_64      2:13.2.4-0.el7.centos       ceph      1.7 M
    [ceph-sh-node1][DEBUG ]  ceph-mgr               x86_64      2:13.2.4-0.el7.centos       ceph      3.4 M
    [ceph-sh-node1][DEBUG ]  ceph-mon               x86_64      2:13.2.4-0.el7.centos       ceph      4.0 M
    [ceph-sh-node1][DEBUG ]  ceph-osd               x86_64      2:13.2.4-0.el7.centos       ceph       13 M
    [ceph-sh-node1][DEBUG ]  ceph-selinux           x86_64      2:13.2.4-0.el7.centos       ceph       19 k
    [ceph-sh-node1][DEBUG ]  gdisk                  x86_64      0.8.10-2.el7                base      189 k
    [ceph-sh-node1][DEBUG ]  libcephfs2             x86_64      2:13.2.4-0.el7.centos       ceph      432 k
    [ceph-sh-node1][DEBUG ]  liboath                x86_64      2.4.1-9.el7                 epel       35 k
    [ceph-sh-node1][DEBUG ]  librados2              x86_64      2:13.2.4-0.el7.centos       ceph      2.8 M
    [ceph-sh-node1][DEBUG ]  libradosstriper1       x86_64      2:13.2.4-0.el7.centos       ceph      327 k
    [ceph-sh-node1][DEBUG ]  librbd1                x86_64      2:13.2.4-0.el7.centos       ceph      1.2 M
    [ceph-sh-node1][DEBUG ]  librgw2                x86_64      2:13.2.4-0.el7.centos       ceph      2.0 M
    [ceph-sh-node1][DEBUG ]  mailcap                noarch      2.1.41-2.el7                base       31 k
    [ceph-sh-node1][DEBUG ]  python-cephfs          x86_64      2:13.2.4-0.el7.centos       ceph       85 k
    [ceph-sh-node1][DEBUG ]  python-cffi            x86_64      1.6.0-5.el7                 base      218 k
    [ceph-sh-node1][DEBUG ]  python-ply             noarch      3.4-11.el7                  base      123 k
    [ceph-sh-node1][DEBUG ]  python-pycparser       noarch      2.14-1.el7                  base      104 k
    [ceph-sh-node1][DEBUG ]  python-rados           x86_64      2:13.2.4-0.el7.centos       ceph      188 k
    [ceph-sh-node1][DEBUG ]  python-rbd             x86_64      2:13.2.4-0.el7.centos       ceph      130 k
    [ceph-sh-node1][DEBUG ]  python-repoze-lru      noarch      0.4-3.el7                   epel       13 k
    [ceph-sh-node1][DEBUG ]  python-rgw             x86_64      2:13.2.4-0.el7.centos       ceph       74 k
    [ceph-sh-node1][DEBUG ]  python-routes          noarch      1.13-2.el7                  epel      640 k
    [ceph-sh-node1][DEBUG ]  python2-bcrypt         x86_64      3.1.4-4.el7                 epel       38 k
    [ceph-sh-node1][DEBUG ]  python2-six            noarch      1.9.0-0.el7                 epel      2.9 k
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] Transaction Summary
    [ceph-sh-node1][DEBUG ] ================================================================================
    [ceph-sh-node1][DEBUG ] Install  2 Packages (+26 Dependent packages)
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] Total download size: 54 M
    [ceph-sh-node1][DEBUG ] Installed size: 178 M
    [ceph-sh-node1][DEBUG ] Downloading packages:
    [ceph-sh-node1][DEBUG ] --------------------------------------------------------------------------------
    [ceph-sh-node1][DEBUG ] Total                                               11 MB/s |  54 MB  00:05     
    [ceph-sh-node1][DEBUG ] Running transaction check
    [ceph-sh-node1][DEBUG ] Running transaction test
    [ceph-sh-node1][DEBUG ] Transaction test succeeded
    [ceph-sh-node1][DEBUG ] Running transaction
    [ceph-sh-node1][DEBUG ]   Installing : 2:librados2-13.2.4-0.el7.centos.x86_64                      1/28 
    [ceph-sh-node1][DEBUG ]   Installing : liboath-2.4.1-9.el7.x86_64                                  2/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:librgw2-13.2.4-0.el7.centos.x86_64                        3/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:libcephfs2-13.2.4-0.el7.centos.x86_64                     4/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:python-rados-13.2.4-0.el7.centos.x86_64                   5/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:librbd1-13.2.4-0.el7.centos.x86_64                        6/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:python-rbd-13.2.4-0.el7.centos.x86_64                     7/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:python-rgw-13.2.4-0.el7.centos.x86_64                     8/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:python-cephfs-13.2.4-0.el7.centos.x86_64                  9/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:libradosstriper1-13.2.4-0.el7.centos.x86_64              10/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-common-13.2.4-0.el7.centos.x86_64                   11/28 
    [ceph-sh-node1][DEBUG ]   Installing : gdisk-0.8.10-2.el7.x86_64                                  12/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-base-13.2.4-0.el7.centos.x86_64                     13/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-selinux-13.2.4-0.el7.centos.x86_64                  14/28 
    [ceph-sh-node1][DEBUG ] /usr/lib/python2.7/site-packages/ceph_disk/main.py:5689: UserWarning: 
    [ceph-sh-node1][DEBUG ] *******************************************************************************
    [ceph-sh-node1][DEBUG ] This tool is now deprecated in favor of ceph-volume.
    [ceph-sh-node1][DEBUG ] It is recommended to use ceph-volume for OSD deployments. For details see:
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ]     http://docs.ceph.com/docs/master/ceph-volume/#migrating
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] *******************************************************************************
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ]   warnings.warn(DEPRECATION_WARNING)
    [ceph-sh-node1][DEBUG ] /usr/lib/python2.7/site-packages/ceph_disk/main.py:5721: UserWarning: 
    [ceph-sh-node1][DEBUG ] *******************************************************************************
    [ceph-sh-node1][DEBUG ] This tool is now deprecated in favor of ceph-volume.
    [ceph-sh-node1][DEBUG ] It is recommended to use ceph-volume for OSD deployments. For details see:
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ]     http://docs.ceph.com/docs/master/ceph-volume/#migrating
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] *******************************************************************************
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ]   warnings.warn(DEPRECATION_WARNING)
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-mon-13.2.4-0.el7.centos.x86_64                      15/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-osd-13.2.4-0.el7.centos.x86_64                      16/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-mds-13.2.4-0.el7.centos.x86_64                      17/28 
    [ceph-sh-node1][DEBUG ]   Installing : python-repoze-lru-0.4-3.el7.noarch                         18/28 
    [ceph-sh-node1][DEBUG ]   Installing : python-routes-1.13-2.el7.noarch                            19/28 
    [ceph-sh-node1][DEBUG ]   Installing : python-ply-3.4-11.el7.noarch                               20/28 
    [ceph-sh-node1][DEBUG ]   Installing : python-pycparser-2.14-1.el7.noarch                         21/28 
    [ceph-sh-node1][DEBUG ]   Installing : python-cffi-1.6.0-5.el7.x86_64                             22/28 
    [ceph-sh-node1][DEBUG ]   Installing : mailcap-2.1.41-2.el7.noarch                                23/28 
    [ceph-sh-node1][DEBUG ]   Installing : python2-six-1.9.0-0.el7.noarch                             24/28 
    [ceph-sh-node1][DEBUG ]   Installing : python2-bcrypt-3.1.4-4.el7.x86_64                          25/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-mgr-13.2.4-0.el7.centos.x86_64                      26/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-13.2.4-0.el7.centos.x86_64                          27/28 
    [ceph-sh-node1][DEBUG ]   Installing : 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64                  28/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:librados2-13.2.4-0.el7.centos.x86_64                      1/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : python2-six-1.9.0-0.el7.noarch                              2/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : mailcap-2.1.41-2.el7.noarch                                 3/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-base-13.2.4-0.el7.centos.x86_64                      4/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:libcephfs2-13.2.4-0.el7.centos.x86_64                     5/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:python-rgw-13.2.4-0.el7.centos.x86_64                     6/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : python-routes-1.13-2.el7.noarch                             7/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:libradosstriper1-13.2.4-0.el7.centos.x86_64               8/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-radosgw-13.2.4-0.el7.centos.x86_64                   9/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-common-13.2.4-0.el7.centos.x86_64                   10/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:python-rados-13.2.4-0.el7.centos.x86_64                  11/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:python-rbd-13.2.4-0.el7.centos.x86_64                    12/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : python2-bcrypt-3.1.4-4.el7.x86_64                          13/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : python-ply-3.4-11.el7.noarch                               14/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-mon-13.2.4-0.el7.centos.x86_64                      15/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:librgw2-13.2.4-0.el7.centos.x86_64                       16/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : python-cffi-1.6.0-5.el7.x86_64                             17/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-mgr-13.2.4-0.el7.centos.x86_64                      18/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : python-repoze-lru-0.4-3.el7.noarch                         19/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:librbd1-13.2.4-0.el7.centos.x86_64                       20/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-13.2.4-0.el7.centos.x86_64                          21/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : python-pycparser-2.14-1.el7.noarch                         22/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-osd-13.2.4-0.el7.centos.x86_64                      23/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : gdisk-0.8.10-2.el7.x86_64                                  24/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-mds-13.2.4-0.el7.centos.x86_64                      25/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:python-cephfs-13.2.4-0.el7.centos.x86_64                 26/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : liboath-2.4.1-9.el7.x86_64                                 27/28 
    [ceph-sh-node1][DEBUG ]   Verifying  : 2:ceph-selinux-13.2.4-0.el7.centos.x86_64                  28/28 
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] Installed:
    [ceph-sh-node1][DEBUG ]   ceph.x86_64 2:13.2.4-0.el7.centos  ceph-radosgw.x86_64 2:13.2.4-0.el7.centos 
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] Dependency Installed:
    [ceph-sh-node1][DEBUG ]   ceph-base.x86_64 2:13.2.4-0.el7.centos                                        
    [ceph-sh-node1][DEBUG ]   ceph-common.x86_64 2:13.2.4-0.el7.centos                                      
    [ceph-sh-node1][DEBUG ]   ceph-mds.x86_64 2:13.2.4-0.el7.centos                                         
    [ceph-sh-node1][DEBUG ]   ceph-mgr.x86_64 2:13.2.4-0.el7.centos                                         
    [ceph-sh-node1][DEBUG ]   ceph-mon.x86_64 2:13.2.4-0.el7.centos                                         
    [ceph-sh-node1][DEBUG ]   ceph-osd.x86_64 2:13.2.4-0.el7.centos                                         
    [ceph-sh-node1][DEBUG ]   ceph-selinux.x86_64 2:13.2.4-0.el7.centos                                     
    [ceph-sh-node1][DEBUG ]   gdisk.x86_64 0:0.8.10-2.el7                                                   
    [ceph-sh-node1][DEBUG ]   libcephfs2.x86_64 2:13.2.4-0.el7.centos                                       
    [ceph-sh-node1][DEBUG ]   liboath.x86_64 0:2.4.1-9.el7                                                  
    [ceph-sh-node1][DEBUG ]   librados2.x86_64 2:13.2.4-0.el7.centos                                        
    [ceph-sh-node1][DEBUG ]   libradosstriper1.x86_64 2:13.2.4-0.el7.centos                                 
    [ceph-sh-node1][DEBUG ]   librbd1.x86_64 2:13.2.4-0.el7.centos                                          
    [ceph-sh-node1][DEBUG ]   librgw2.x86_64 2:13.2.4-0.el7.centos                                          
    [ceph-sh-node1][DEBUG ]   mailcap.noarch 0:2.1.41-2.el7                                                 
    [ceph-sh-node1][DEBUG ]   python-cephfs.x86_64 2:13.2.4-0.el7.centos                                    
    [ceph-sh-node1][DEBUG ]   python-cffi.x86_64 0:1.6.0-5.el7                                              
    [ceph-sh-node1][DEBUG ]   python-ply.noarch 0:3.4-11.el7                                                
    [ceph-sh-node1][DEBUG ]   python-pycparser.noarch 0:2.14-1.el7                                          
    [ceph-sh-node1][DEBUG ]   python-rados.x86_64 2:13.2.4-0.el7.centos                                     
    [ceph-sh-node1][DEBUG ]   python-rbd.x86_64 2:13.2.4-0.el7.centos                                       
    [ceph-sh-node1][DEBUG ]   python-repoze-lru.noarch 0:0.4-3.el7                                          
    [ceph-sh-node1][DEBUG ]   python-rgw.x86_64 2:13.2.4-0.el7.centos                                       
    [ceph-sh-node1][DEBUG ]   python-routes.noarch 0:1.13-2.el7                                             
    [ceph-sh-node1][DEBUG ]   python2-bcrypt.x86_64 0:3.1.4-4.el7                                           
    [ceph-sh-node1][DEBUG ]   python2-six.noarch 0:1.9.0-0.el7                                              
    [ceph-sh-node1][DEBUG ] 
    [ceph-sh-node1][DEBUG ] Complete!
    [ceph-sh-node1][INFO  ] Running command: sudo ceph --version
    [ceph-sh-node1][DEBUG ] ceph version 13.2.4 (b10be4d44915a4d78a8e06aa31919e74927b142e) mimic (stable)
    ```

6. installing a build
    
    ```
    ls -l | grep "^-" | wc -l
    sudo cp -rfdp /usr/local/lib64/python2.7/site-packages/* /usr/lib64/python2.7
    sudo cp -rfdp /usr/local/lib64/lib*  /usr/lib64
    sudo cp -rfdp /usr/local/lib/python2.7/site-packages/* /usr/lib64/python2.7
    ```
    ```
    cd build
    make install
    [ 68%] Built target unittest_shared_cache
    [ 68%] Built target unittest_sharedptr_registry
    [ 68%] Built target unittest_shunique_lock
    [ 68%] Built target unittest_sloppy_crc_map
    [ 68%] Built target unittest_str_map
    [ 68%] Built target unittest_tableformatter
    [ 68%] Built target unittest_throttle
    [ 68%] Built target unittest_time
    [ 68%] Built target unittest_url_escape
    [ 69%] Built target unittest_util
    [ 69%] Built target unittest_weighted_priority_queue
    [ 69%] Built target unittest_xmlformatter
    [ 69%] Built target ceph_example
    [ 69%] Built target unittest_compression
    [ 69%] Built target unittest_crush
    [ 69%] Built target unittest_crush_wrapper
    [ 69%] Built target unittest_direct_messenger
    [ 69%] Built target ceph_erasure_code
    [ 69%] Built target ceph_erasure_code_benchmark
    [ 69%] Built target ceph_erasure_code_non_regression
    [ 69%] Built target ec_example
    [ 69%] Built target ec_fail_to_initialize
    [ 69%] Built target ec_fail_to_register
    [ 69%] Built target ec_hangs
    [ 69%] Built target ec_missing_entry_point
    [ 69%] Built target ec_missing_version
    [ 69%] Built target unittest_erasure_code
    [ 69%] Built target unittest_erasure_code_example
    [ 69%] Built target unittest_erasure_code_isa
    [ 69%] Built target unittest_erasure_code_jerasure
    [ 69%] Built target unittest_erasure_code_lrc
    [ 69%] Built target unittest_erasure_code_plugin
    [ 69%] Built target unittest_erasure_code_plugin_isa
    [ 69%] Built target unittest_erasure_code_plugin_jerasure
    [ 69%] Built target unittest_erasure_code_plugin_lrc
    [ 70%] Built target unittest_erasure_code_plugin_shec
    [ 70%] Built target unittest_erasure_code_shec
    [ 70%] Built target unittest_erasure_code_shec_all
    [ 70%] Built target unittest_erasure_code_shec_arguments
    [ 70%] Built target unittest_erasure_code_shec_thread
    [ 70%] Built target ceph_test_filestore
    [ 70%] Built target ceph_test_trim_caps
    [ 70%] Built target unittest_mds_types
    [ 70%] Built target journal_test_mock
    [ 71%] Built target rados_test_stub
    [ 72%] Built target unittest_journal
    [ 73%] Built target ceph_test_libcephfs
    [ 73%] Built target ceph_test_libcephfs_access
    [ 73%] Built target ceph_test_rados_api_aio
    [ 73%] Built target ceph_test_rados_api_c_read_operations
    [ 73%] Built target ceph_test_rados_api_c_write_operations
    [ 73%] Built target ceph_test_rados_api_cls
    [ 73%] Built target ceph_test_rados_api_cmd
    [ 73%] Built target ceph_test_rados_api_io
    [ 73%] Built target ceph_test_rados_api_list
    [ 73%] Built target ceph_test_rados_api_lock
    [ 73%] Built target ceph_test_rados_api_misc
    [ 73%] Built target ceph_test_rados_api_pool
    [ 74%] Built target ceph_test_rados_api_service
    [ 74%] Built target ceph_test_rados_api_snapshots
    [ 74%] Built target ceph_test_rados_api_stat
    [ 74%] Built target ceph_test_rados_api_tier
    [ 74%] Built target ceph_test_rados_api_watch_notify
    [ 74%] Built target unittest_librados
    [ 74%] Built target unittest_librados_config
    [ 74%] Built target rados_striper_test
    [ 74%] Built target ceph_test_rados_striper_api_aio
    [ 74%] Built target ceph_test_rados_striper_api_io
    [ 74%] Built target ceph_test_rados_striper_api_striping
    [ 74%] Built target rbd_test
    [ 74%] Built target rbd_api
    [ 74%] Built target ceph_test_librbd
    [ 74%] Built target ceph_test_librbd_api
    [ 74%] Built target ceph_test_librbd_fsx
    [ 75%] Built target rbd_test_mock
    [ 78%] Built target unittest_librbd
    [ 78%] Built target simple_client
    [ 78%] Built target simple_server
    [ 78%] Built target unittest_mds_authcap
    [ 78%] Built target unittest_mds_sessionfilter
    [ 78%] Built target ceph_test_mon_msg
    [ 78%] Built target ceph_test_mon_workloadgen
    [ 78%] Built target unittest_mon_moncap
    [ 78%] Built target unittest_mon_montypes
    [ 78%] Built target unittest_mon_pgmap
    [ 78%] Built target ceph_perf_msgr_client
    [ 78%] Built target ceph_perf_msgr_server
    [ 78%] Built target ceph_test_async_driver
    [ 78%] Built target ceph_test_async_networkstack
    [ 78%] Built target ceph_test_msgr
    [ 78%] Built target ceph_test_keyvaluedb_atomicity
    [ 78%] Built target ceph_test_keyvaluedb_iterators
    [ 78%] Built target ceph_test_object_map
    [ 78%] Built target ceph_perf_objectstore
    [ 78%] Built target ceph_test_filestore_idempotent
    [ 79%] Built target ceph_test_filestore_idempotent_sequence
    [ 79%] Built target ceph_test_keyvaluedb
    [ 79%] Built target store_test_fixture
    [ 79%] Built target ceph_test_objectstore
    [ 79%] Built target ceph_test_objectstore_workloadgen
    [ 79%] Built target unittest_alloc
    [ 80%] Built target unittest_bit_alloc
    [ 80%] Built target unittest_bluefs
    [ 80%] Built target unittest_bluestore_types
    [ 80%] Built target unittest_chain_xattr
    [ 80%] Built target unittest_memstore_clone
    [ 80%] Built target unittest_rocksdb_option
    [ 80%] Built target unittest_transaction
    [ 80%] Built target unittest_lfnindex
    [ 81%] Built target ceph_test_rados
    [ 81%] Built target unittest_ec_transaction
    [ 81%] Built target unittest_ecbackend
    [ 81%] Built target unittest_extent_cache
    [ 81%] Built target unittest_hitset
    [ 81%] Built target unittest_mclock_client_queue
    [ 81%] Built target unittest_mclock_op_class_queue
    [ 81%] Built target unittest_osd_osdcap
    [ 81%] Built target unittest_osd_types
    [ 81%] Built target unittest_osdmap
    [ 81%] Built target unittest_osdscrub
    [ 81%] Built target unittest_pg_transaction
    [ 81%] Built target unittest_pglog
    [ 81%] Built target ceph_test_objectcacher_stress
    [ 82%] Built target test_rgw_a
    [ 82%] Built target ceph_test_rgw_manifest
    [ 82%] Built target ceph_test_rgw_obj
    [ 83%] Built target unittest_http_manager
    [ 83%] Built target unittest_rgw_bencode
    [ 83%] Built target unittest_rgw_compression
    [ 83%] Built target unittest_rgw_crypto
    [ 83%] Built target unittest_rgw_iam_policy
    [ 83%] Built target unittest_rgw_period_history
    [ 83%] Built target unittest_rgw_string
    [ 84%] Built target rbd_mirror_test
    [ 86%] Built target rbd_mirror_internal
    [ 86%] Built target ceph_test_rbd_mirror
    [ 86%] Built target ceph_test_rbd_mirror_random_write
    [ 87%] Built target unittest_rbd_mirror
    [ 88%] Built target systest
    [ 88%] Built target ceph_test_rados_delete_pools_parallel
    [ 88%] Built target ceph_test_rados_list_parallel
    [ 88%] Built target ceph_test_rados_open_pools_parallel
    [ 89%] Built target ceph_test_rados_watch_notify
    [ 89%] Built target ceph-authtool
    [ 89%] Built target ceph-client-debug
    [ 89%] Built target ceph-conf
    [ 89%] Built target ceph-kvstore-tool
    [ 89%] Built target ceph-monstore-tool
    [ 90%] Built target ceph-objectstore-tool
    [ 90%] Built target ceph-osdomap-tool
    [ 90%] Built target ceph_psim
    [ 90%] Built target ceph_radosacl
    [ 90%] Built target ceph_scratchtool
    [ 90%] Built target ceph_scratchtoolpp
    [ 91%] Built target crushtool
    [ 92%] Built target monmaptool
    [ 92%] Built target osdmaptool
    [ 92%] Built target cephfs-data-scan
    [ 93%] Built target cephfs-journal-tool
    [ 93%] Built target cephfs-table-tool
    [ 96%] Built target rbd
    [ 96%] Built target rbd-mirror
    [ 96%] Built target rbd-nbd
    [ 97%] Built target isal_crypto_plugin_objs
    [ 97%] Built target rbd-fuse
    [ 97%] Built target rbd-replay
    [ 97%] Built target rbd-replay-prep
    [ 98%] Built target cls_kvs
    [ 98%] Built target ceph_rgw_jsonparser
    [ 98%] Built target ceph_rgw_multiparser
    [ 98%] Built target radosgw_a
    [ 98%] Built target radosgw
    [ 98%] Built target radosgw-admin
    [ 98%] Built target radosgw-es
    [ 98%] Built target radosgw-object-expirer
    [100%] Built target radosgw-token
    [100%] manpages building
    [100%] Built target manpages
    Install the project...
    -- Install configuration: "RelWithDebInfo"
    -- Installing: /usr/local/lib64/ceph/libceph-common.so.0
    -- Installing: /usr/local/lib64/ceph/libceph-common.so
    -- Installing: /usr/local/bin/ceph-mgr
    -- Set runtime path of "/usr/local/bin/ceph-mgr" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/librados-config
    -- Set runtime path of "/usr/local/bin/librados-config" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-dencoder
    -- Set runtime path of "/usr/local/bin/ceph-dencoder" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-mon
    -- Set runtime path of "/usr/local/bin/ceph-mon" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-osd
    -- Set runtime path of "/usr/local/bin/ceph-osd" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-mds
    -- Set runtime path of "/usr/local/bin/ceph-mds" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-debugpack
    -- Installing: /usr/local/bin/ceph-coverage
    -- Installing: /usr/local/bin/ceph
    -- Installing: /usr/local/bin/ceph-crush-location
    -- Installing: /usr/local/bin/ceph-post-file
    -- Installing: /usr/local/bin/ceph-run
    -- Installing: /usr/local/bin/ceph-rest-api
    -- Installing: /usr/local/bin/ceph-clsinfo
    -- Installing: /usr/local/etc/init.d/ceph
    -- Installing: /usr/local/share/ceph/id_rsa_drop.ceph.com
    -- Installing: /usr/local/share/ceph/id_rsa_drop.ceph.com.pub
    -- Installing: /usr/local/share/ceph/known_hosts_drop.ceph.com
    -- Installing: /usr/local/libexec/ceph/ceph_common.sh
    -- Installing: /usr/local/libexec/ceph/ceph-osd-prestart.sh
    -- Installing: /usr/local/sbin/ceph-create-keys
    -- Installing: /usr/local/lib64/libcephfs.so.2.0.0
    -- Installing: /usr/local/lib64/libcephfs.so.2
    -- Installing: /usr/local/lib64/libcephfs.so
    -- Set runtime path of "/usr/local/lib64/libcephfs.so.2.0.0" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/include/cephfs
    -- Installing: /usr/local/include/cephfs/ceph_statx.h
    -- Installing: /usr/local/include/cephfs/libcephfs.h
    -- Installing: /usr/local/bin/ceph-syn
    -- Set runtime path of "/usr/local/bin/ceph-syn" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/sbin/mount.ceph
    -- Installing: /usr/local/bin/ceph-fuse
    -- Set runtime path of "/usr/local/bin/ceph-fuse" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/sbin/mount.fuse.ceph
    -- Installing: /usr/local/bin/ceph-rbdnamer
    -- Installing: /usr/local/bin/rbd-replay-many
    -- Installing: /usr/local/bin/rbdmap
    -- Installing: /usr/local/share/doc/ceph/sample.ceph.conf
    -- Installing: /usr/local/lib64/librados_tp.so.2.0.0
    -- Installing: /usr/local/lib64/librados_tp.so.2
    -- Installing: /usr/local/lib64/librados_tp.so
    -- Installing: /usr/local/lib64/libosd_tp.so.1.0.0
    -- Installing: /usr/local/lib64/libosd_tp.so.1
    -- Installing: /usr/local/lib64/libosd_tp.so
    -- Installing: /usr/local/lib64/libos_tp.so.1.0.0
    -- Installing: /usr/local/lib64/libos_tp.so.1
    -- Installing: /usr/local/lib64/libos_tp.so
    -- Installing: /usr/local/lib64/librbd_tp.so.1.0.0
    -- Installing: /usr/local/lib64/librbd_tp.so.1
    -- Installing: /usr/local/lib64/librbd_tp.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_sdk.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_sdk.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_sdk.so
    -- Set runtime path of "/usr/local/lib64/rados-classes/libcls_sdk.so.1.0.0" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib64/rados-classes/libcls_hello.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_hello.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_hello.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_numops.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_numops.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_numops.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_rbd.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_rbd.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_rbd.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_lock.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_lock.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_lock.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_refcount.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_refcount.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_refcount.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_version.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_version.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_version.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_log.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_log.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_log.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_statelog.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_statelog.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_statelog.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_timeindex.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_timeindex.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_timeindex.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_replica_log.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_replica_log.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_replica_log.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_user.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_user.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_user.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_journal.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_journal.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_journal.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_rgw.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_rgw.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_rgw.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_cephfs.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_cephfs.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_cephfs.so
    -- Installing: /usr/local/lib64/rados-classes/libcls_lua.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_lua.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_lua.so
    -- Installing: /usr/local/include/rados/librados.h
    -- Installing: /usr/local/include/rados/rados_types.h
    -- Installing: /usr/local/include/rados/rados_types.hpp
    -- Installing: /usr/local/include/rados/librados.hpp
    -- Installing: /usr/local/include/rados/buffer.h
    -- Installing: /usr/local/include/rados/buffer_fwd.h
    -- Installing: /usr/local/include/rados/inline_memory.h
    -- Installing: /usr/local/include/rados/memory.h
    -- Installing: /usr/local/include/rados/page.h
    -- Installing: /usr/local/include/rados/crc32c.h
    -- Installing: /usr/local/include/rados/objclass.h
    -- Installing: /usr/local/include/radosstriper/libradosstriper.h
    -- Installing: /usr/local/include/radosstriper/libradosstriper.hpp
    -- Installing: /usr/local/include/rbd/features.h
    -- Installing: /usr/local/include/rbd/librbd.h
    -- Installing: /usr/local/include/rbd/librbd.hpp
    -- Installing: /usr/local/include/rados/librgw.h
    -- Installing: /usr/local/include/rados/rgw_file.h
    -- Installing: /usr/local/lib64/librados.so.2.0.0
    -- Installing: /usr/local/lib64/librados.so.2
    -- Installing: /usr/local/lib64/librados.so
    -- Set runtime path of "/usr/local/lib64/librados.so.2.0.0" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib64/libradosstriper.so.1.0.0
    -- Installing: /usr/local/lib64/libradosstriper.so.1
    -- Installing: /usr/local/lib64/libradosstriper.so
    -- Set runtime path of "/usr/local/lib64/libradosstriper.so.1.0.0" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib/python2.7/site-packages/ceph_argparse.py
    -- Installing: /usr/local/lib/python2.7/site-packages/ceph_daemon.py
    -- Installing: /usr/local/lib/python2.7/site-packages/ceph_volume_client.py
    -- Installing: /usr/local/lib/python2.7/site-packages/ceph_rest_api.py
    -- Installing: /usr/local/lib64/ceph/mgr
    -- Installing: /usr/local/lib64/ceph/mgr/balancer
    -- Installing: /usr/local/lib64/ceph/mgr/balancer/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/balancer/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/HACKING.rst
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/README.rst
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/base.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/cephfs_clients.py
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/clients.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/config_options.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/filesystem.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/health.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/osd_perf.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/osds.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/rbd_iscsi.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/rbd_iscsi.py
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/rbd_ls.py
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/rbd_mirroring.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/rbd_mirroring.py
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/rbd_pool.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/remote_view_cache.py
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/servers.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/standby.html
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/.gitignore
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/.jshintrc
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/LICENSE
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/README.md
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap/css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap/css/bootstrap.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap/css/bootstrap.min.css.map
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap/fonts
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap/fonts/glyphicons-halflings-regular.woff
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap/fonts/glyphicons-halflings-regular.woff2
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap/js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/bootstrap/js/bootstrap.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/AdminLTE.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/_all-skins.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-black-light.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-black.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-blue-light.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-blue.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-green-light.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-green.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-purple-light.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-purple.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-red-light.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-red.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-yellow-light.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/css/skins/skin-yellow.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/img
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/img/boxed-bg.jpg
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/img/boxed-bg.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/img/default-50x50.gif
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/img/icons.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/dist/js/app.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/chartjs
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/chartjs/Chart.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/chartjs/Chart.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/dataTables.bootstrap.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/images
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/images/sort_asc.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/images/sort_asc_disabled.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/images/sort_both.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/images/sort_desc.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/images/sort_desc_disabled.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/jquery.dataTables.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/jquery.dataTables.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/datatables/jquery.dataTables_themeroller.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/ionslider
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/ionslider/img
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/ionslider/img/sprite-skin-flat.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/ionslider/img/sprite-skin-nice.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/ionslider/ion.rangeSlider.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/ionslider/ion.rangeSlider.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/ionslider/ion.rangeSlider.skinFlat.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/ionslider/ion.rangeSlider.skinNice.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/jQuery
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/jQuery/jquery-2.2.3.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/sparkline
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/sparkline/jquery.sparkline.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/AdminLTE-2.3.7/plugins/sparkline/jquery.sparkline.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/Ceph_Logo_Standard_RGB_White_120411_fa.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/favicon.ico
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/Chart.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/Chart.js/2.4.0
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/Chart.js/2.4.0/Chart.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/Chart.js/LICENSE.md
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome/4.7.0
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome/4.7.0/HELP-US-OUT.txt
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome/4.7.0/css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome/4.7.0/css/font-awesome.min.css
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome/4.7.0/fonts
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome/4.7.0/fonts/fontawesome-webfont.woff
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome/4.7.0/fonts/fontawesome-webfont.woff2
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/font-awesome/COPYING
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/moment.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/moment.js/2.17.1
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/moment.js/2.17.1/moment.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/rivets
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/rivets/0.9.6
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/rivets/0.9.6/rivets.bundled.min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/underscore.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/underscore.js/1.8.3
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/libs/underscore.js/1.8.3/underscore-min.js
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/static/logo-mini.png
    -- Installing: /usr/local/lib64/ceph/mgr/dashboard/types.py
    -- Installing: /usr/local/lib64/ceph/mgr/influx
    -- Installing: /usr/local/lib64/ceph/mgr/influx/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/influx/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/localpool
    -- Installing: /usr/local/lib64/ceph/mgr/localpool/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/localpool/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/prometheus
    -- Installing: /usr/local/lib64/ceph/mgr/prometheus/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/prometheus/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/config.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/crush.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/doc.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/mon.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/osd.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/pool.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/request.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/api/server.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/common.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/decorators.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/hooks.py
    -- Installing: /usr/local/lib64/ceph/mgr/restful/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/selftest
    -- Installing: /usr/local/lib64/ceph/mgr/selftest/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/selftest/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/status
    -- Installing: /usr/local/lib64/ceph/mgr/status/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/status/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/zabbix
    -- Installing: /usr/local/lib64/ceph/mgr/zabbix/__init__.py
    -- Installing: /usr/local/lib64/ceph/mgr/zabbix/module.py
    -- Installing: /usr/local/lib64/ceph/mgr/zabbix/zabbix_template.xml
    -- Installing: /usr/local/lib64/ceph/mgr/.gitignore
    -- Installing: /usr/local/lib64/ceph/mgr/mgr_module.py
    running build
    running build_ext
    cythoning rados.pyx to /usr/local/src/ceph/build/src/pybind/rados/pyrex/rados.c
    creating /usr/local/src/ceph/build/src/pybind/rados/pyrex
    building 'rados' extension
    creating /usr/local/src/ceph/build/src/pybind/rados/usr
    creating /usr/local/src/ceph/build/src/pybind/rados/usr/local
    creating /usr/local/src/ceph/build/src/pybind/rados/usr/local/src
    creating /usr/local/src/ceph/build/src/pybind/rados/usr/local/src/ceph
    creating /usr/local/src/ceph/build/src/pybind/rados/usr/local/src/ceph/build
    creating /usr/local/src/ceph/build/src/pybind/rados/usr/local/src/ceph/build/src
    creating /usr/local/src/ceph/build/src/pybind/rados/usr/local/src/ceph/build/src/pybind
    creating /usr/local/src/ceph/build/src/pybind/rados/usr/local/src/ceph/build/src/pybind/rados
    creating /usr/local/src/ceph/build/src/pybind/rados/usr/local/src/ceph/build/src/pybind/rados/pyrex
    /usr/bin/cc -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -iquote/usr/local/src/ceph/src/include -fPIC -I/usr/include/python2.7 -I/usr/include/python2.7 -I/usr/include/python2.7 -c /usr/local/src/ceph/build/src/pybind/rados/pyrex/rados.c -o /usr/local/src/ceph/build/src/pybind/rados/usr/local/src/ceph/build/src/pybind/rados/pyrex/rados.o -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -lpthread -ldl -lutil -lm -lpython2.7 -Xlinker -export-dynamic
    gcc -pthread -shared -Wl,-z,relro -L/usr/local/src/ceph/build/lib -iquote/usr/local/src/ceph/src/include /usr/local/src/ceph/build/src/pybind/rados/usr/local/src/ceph/build/src/pybind/rados/pyrex/rados.o -L/usr/lib64 -lrados -lpthread -ldl -lutil -lm -lpython2.7 -lpython2.7 -o /usr/local/src/ceph/build/lib/cython_modules/lib.2/rados.so
    running install
    running install_lib
    creating /usr/local/lib64/python2.7
    creating /usr/local/lib64/python2.7/site-packages
    copying /usr/local/src/ceph/build/lib/cython_modules/lib.2/rgw.so -> /usr/local/lib64/python2.7/site-packages
    copying /usr/local/src/ceph/build/lib/cython_modules/lib.2/rbd.so -> /usr/local/lib64/python2.7/site-packages
    copying /usr/local/src/ceph/build/lib/cython_modules/lib.2/cephfs.so -> /usr/local/lib64/python2.7/site-packages
    copying /usr/local/src/ceph/build/lib/cython_modules/lib.2/rados.so -> /usr/local/lib64/python2.7/site-packages
    running install_egg_info
    running egg_info
    creating /usr/local/src/ceph/build/src/pybind/rados/rados.egg-info
    writing /usr/local/src/ceph/build/src/pybind/rados/rados.egg-info/PKG-INFO
    writing top-level names to /usr/local/src/ceph/build/src/pybind/rados/rados.egg-info/top_level.txt
    writing dependency_links to /usr/local/src/ceph/build/src/pybind/rados/rados.egg-info/dependency_links.txt
    writing manifest file '/usr/local/src/ceph/build/src/pybind/rados/rados.egg-info/SOURCES.txt'
    reading manifest file '/usr/local/src/ceph/build/src/pybind/rados/rados.egg-info/SOURCES.txt'
    reading manifest template 'MANIFEST.in'
    writing manifest file '/usr/local/src/ceph/build/src/pybind/rados/rados.egg-info/SOURCES.txt'
    Copying /usr/local/src/ceph/build/src/pybind/rados/rados.egg-info to /usr/local/lib64/python2.7/site-packages/rados-2.0.0-py2.7.egg-info
    running install_scripts
    writing list of installed files to '/dev/null'
    running build
    running build_ext
    cythoning rbd.pyx to /usr/local/src/ceph/build/src/pybind/rbd/pyrex/rbd.c
    creating /usr/local/src/ceph/build/src/pybind/rbd/pyrex
    building 'rbd' extension
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr/local
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src/ceph
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src/ceph/build
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src/ceph/build/src
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src/ceph/build/src/pybind
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src/ceph/build/src/pybind/rbd
    creating /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src/ceph/build/src/pybind/rbd/pyrex
    /usr/bin/cc -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -iquote/usr/local/src/ceph/src/include -fPIC -I/usr/include/python2.7 -I/usr/include/python2.7 -I/usr/include/python2.7 -c /usr/local/src/ceph/build/src/pybind/rbd/pyrex/rbd.c -o /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src/ceph/build/src/pybind/rbd/pyrex/rbd.o -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -lpthread -ldl -lutil -lm -lpython2.7 -Xlinker -export-dynamic
    gcc -pthread -shared -Wl,-z,relro -L/usr/local/src/ceph/build/lib -iquote/usr/local/src/ceph/src/include /usr/local/src/ceph/build/src/pybind/rbd/usr/local/src/ceph/build/src/pybind/rbd/pyrex/rbd.o -L/usr/lib64 -lrbd -lrados -lpthread -ldl -lutil -lm -lpython2.7 -lpython2.7 -o /usr/local/src/ceph/build/lib/cython_modules/lib.2/rbd.so
    running install
    running install_lib
    copying /usr/local/src/ceph/build/lib/cython_modules/lib.2/rbd.so -> /usr/local/lib64/python2.7/site-packages
    running install_egg_info
    running egg_info
    creating /usr/local/src/ceph/build/src/pybind/rbd/rbd.egg-info
    writing /usr/local/src/ceph/build/src/pybind/rbd/rbd.egg-info/PKG-INFO
    writing top-level names to /usr/local/src/ceph/build/src/pybind/rbd/rbd.egg-info/top_level.txt
    writing dependency_links to /usr/local/src/ceph/build/src/pybind/rbd/rbd.egg-info/dependency_links.txt
    writing manifest file '/usr/local/src/ceph/build/src/pybind/rbd/rbd.egg-info/SOURCES.txt'
    reading manifest file '/usr/local/src/ceph/build/src/pybind/rbd/rbd.egg-info/SOURCES.txt'
    reading manifest template 'MANIFEST.in'
    writing manifest file '/usr/local/src/ceph/build/src/pybind/rbd/rbd.egg-info/SOURCES.txt'
    Copying /usr/local/src/ceph/build/src/pybind/rbd/rbd.egg-info to /usr/local/lib64/python2.7/site-packages/rbd-2.0.0-py2.7.egg-info
    running install_scripts
    writing list of installed files to '/dev/null'
    running build
    running build_ext
    cythoning cephfs.pyx to /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c
    creating /usr/local/src/ceph/build/src/pybind/cephfs/pyrex
    building 'cephfs' extension
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr/local
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src/ceph
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src/ceph/build
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src/ceph/build/src
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src/ceph/build/src/pybind
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src/ceph/build/src/pybind/cephfs
    creating /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src/ceph/build/src/pybind/cephfs/pyrex
    /usr/bin/cc -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -iquote/usr/local/src/ceph/src/include -fPIC -I/usr/include/python2.7 -I/usr/include/python2.7 -I/usr/include/python2.7 -c /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c -o /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.o -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -lpthread -ldl -lutil -lm -lpython2.7 -Xlinker -export-dynamic
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c: In function ?._pyx_pw_6cephfs_9LibCephFS_5create_with_rados?.
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:3257:7: warning: variable ?._pyx_clineno?.set but not used [-Wunused-but-set-variable]
    int __pyx_clineno = 0;
        ^
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:3256:15: warning: variable ?._pyx_filename?.set but not used [-Wunused-but-set-variable]
    const char *__pyx_filename = NULL;
                ^
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:3255:7: warning: variable ?._pyx_lineno?.set but not used [-Wunused-but-set-variable]
    int __pyx_lineno = 0;
        ^
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c: In function ?._pyx_pw_6cephfs_9LibCephFS_45readdir?.
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:6898:7: warning: variable ?._pyx_clineno?.set but not used [-Wunused-but-set-variable]
    int __pyx_clineno = 0;
        ^
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:6897:15: warning: variable ?._pyx_filename?.set but not used [-Wunused-but-set-variable]
    const char *__pyx_filename = NULL;
                ^
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:6896:7: warning: variable ?._pyx_lineno?.set but not used [-Wunused-but-set-variable]
    int __pyx_lineno = 0;
        ^
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c: In function ?._pyx_pw_6cephfs_9LibCephFS_47closedir?.
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:7113:7: warning: variable ?._pyx_clineno?.set but not used [-Wunused-but-set-variable]
    int __pyx_clineno = 0;
        ^
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:7112:15: warning: variable ?._pyx_filename?.set but not used [-Wunused-but-set-variable]
    const char *__pyx_filename = NULL;
                ^
    /usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.c:7111:7: warning: variable ?._pyx_lineno?.set but not used [-Wunused-but-set-variable]
    int __pyx_lineno = 0;
        ^
    gcc -pthread -shared -Wl,-z,relro -L/usr/local/src/ceph/build/lib -iquote/usr/local/src/ceph/src/include /usr/local/src/ceph/build/src/pybind/cephfs/usr/local/src/ceph/build/src/pybind/cephfs/pyrex/cephfs.o -L/usr/lib64 -lcephfs -lpthread -ldl -lutil -lm -lpython2.7 -lpython2.7 -o /usr/local/src/ceph/build/lib/cython_modules/lib.2/cephfs.so
    running install
    running install_lib
    copying /usr/local/src/ceph/build/lib/cython_modules/lib.2/cephfs.so -> /usr/local/lib64/python2.7/site-packages
    running install_egg_info
    running egg_info
    creating /usr/local/src/ceph/build/src/pybind/cephfs/cephfs.egg-info
    writing /usr/local/src/ceph/build/src/pybind/cephfs/cephfs.egg-info/PKG-INFO
    writing top-level names to /usr/local/src/ceph/build/src/pybind/cephfs/cephfs.egg-info/top_level.txt
    writing dependency_links to /usr/local/src/ceph/build/src/pybind/cephfs/cephfs.egg-info/dependency_links.txt
    writing manifest file '/usr/local/src/ceph/build/src/pybind/cephfs/cephfs.egg-info/SOURCES.txt'
    reading manifest file '/usr/local/src/ceph/build/src/pybind/cephfs/cephfs.egg-info/SOURCES.txt'
    reading manifest template 'MANIFEST.in'
    writing manifest file '/usr/local/src/ceph/build/src/pybind/cephfs/cephfs.egg-info/SOURCES.txt'
    Copying /usr/local/src/ceph/build/src/pybind/cephfs/cephfs.egg-info to /usr/local/lib64/python2.7/site-packages/cephfs-2.0.0-py2.7.egg-info
    running install_scripts
    writing list of installed files to '/dev/null'
    running build
    running build_ext
    cythoning rgw.pyx to /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c
    creating /usr/local/src/ceph/build/src/pybind/rgw/pyrex
    building 'rgw' extension
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr/local
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src/ceph
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src/ceph/build
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src/ceph/build/src
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src/ceph/build/src/pybind
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src/ceph/build/src/pybind/rgw
    creating /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src/ceph/build/src/pybind/rgw/pyrex
    /usr/bin/cc -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -iquote/usr/local/src/ceph/src/include -fPIC -I/usr/include/python2.7 -I/usr/include/python2.7 -I/usr/include/python2.7 -c /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c -o /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.o -fno-strict-aliasing -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -DNDEBUG -O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -D_GNU_SOURCE -fPIC -fwrapv -lpthread -ldl -lutil -lm -lpython2.7 -Xlinker -export-dynamic
    /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c: In function ?._pyx_pf_3rgw_8LibRGWFS_28readdir?.
    /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c:5667:9: warning: passing argument 4 of ?.gw_readdir?.from incompatible pointer type [enabled by default]
            __pyx_t_5 = rgw_readdir(__pyx_v_self->fs, __pyx_v__dir_handler, (&__pyx_v__offset), (&__pyx_f_3rgw_readdir_cb), ((void *)__pyx_v_iterate_cb), (&__pyx_v__eof), __pyx_v__flags); if (unlikely(__pyx_t_5 == -9000 && __Pyx_ErrOccurredWithGIL())) {__pyx_filename = __pyx_f[0]; __pyx_lineno = 528; __pyx_clineno = __LINE__; goto __pyx_L4;}
            ^
    In file included from /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c:310:0:
    /usr/local/src/ceph/src/include/rados/rgw_file.h:219:5: note: expected ?.gw_readdir_cb?.but argument is of type ?.Bool (*)(const char *, void *, uint64_t)?
    int rgw_readdir(struct rgw_fs *rgw_fs,
        ^
    /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c: In function ?._pyx_pw_3rgw_8LibRGWFS_31fstat?.
    /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c:5764:7: warning: variable ?._pyx_clineno?.set but not used [-Wunused-but-set-variable]
    int __pyx_clineno = 0;
        ^
    /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c:5763:15: warning: variable ?._pyx_filename?.set but not used [-Wunused-but-set-variable]
    const char *__pyx_filename = NULL;
                ^
    /usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.c:5762:7: warning: variable ?._pyx_lineno?.set but not used [-Wunused-but-set-variable]
    int __pyx_lineno = 0;
        ^
    gcc -pthread -shared -Wl,-z,relro -L/usr/local/src/ceph/build/lib -iquote/usr/local/src/ceph/src/include /usr/local/src/ceph/build/src/pybind/rgw/usr/local/src/ceph/build/src/pybind/rgw/pyrex/rgw.o -L/usr/lib64 -lrados -lrgw -lpthread -ldl -lutil -lm -lpython2.7 -lpython2.7 -o /usr/local/src/ceph/build/lib/cython_modules/lib.2/rgw.so
    running install
    running install_lib
    copying /usr/local/src/ceph/build/lib/cython_modules/lib.2/rgw.so -> /usr/local/lib64/python2.7/site-packages
    running install_egg_info
    running egg_info
    creating /usr/local/src/ceph/build/src/pybind/rgw/rgw.egg-info
    writing /usr/local/src/ceph/build/src/pybind/rgw/rgw.egg-info/PKG-INFO
    writing top-level names to /usr/local/src/ceph/build/src/pybind/rgw/rgw.egg-info/top_level.txt
    writing dependency_links to /usr/local/src/ceph/build/src/pybind/rgw/rgw.egg-info/dependency_links.txt
    writing manifest file '/usr/local/src/ceph/build/src/pybind/rgw/rgw.egg-info/SOURCES.txt'
    reading manifest file '/usr/local/src/ceph/build/src/pybind/rgw/rgw.egg-info/SOURCES.txt'
    reading manifest template 'MANIFEST.in'
    writing manifest file '/usr/local/src/ceph/build/src/pybind/rgw/rgw.egg-info/SOURCES.txt'
    Copying /usr/local/src/ceph/build/src/pybind/rgw/rgw.egg-info to /usr/local/lib64/python2.7/site-packages/rgw-2.0.0-py2.7.egg-info
    running install_scripts
    writing list of installed files to '/dev/null'
    running install
    Checking .pth file support in /usr/local/lib/python2.7/site-packages/
    /usr/bin/python2.7 -E -c pass
    TEST FAILED: /usr/local/lib/python2.7/site-packages/ does NOT support .pth files
    error: bad install directory or PYTHONPATH

    You are attempting to install a package to a directory that is not
    on PYTHONPATH and which Python does not read ".pth" files from.  The
    installation directory you specified (via --install-dir, --prefix, or
    the distutils default setting) was:

        /usr/local/lib/python2.7/site-packages/

    and your PYTHONPATH environment variable currently contains:

        ''

    Here are some of your options for correcting the problem:

    \* You can choose a different installation directory, i.e., one that is
    on PYTHONPATH or supports .pth files
    
    \* You can add the installation directory to the PYTHONPATH environment
    variable.  (It must then also be on PYTHONPATH whenever you run
    Python and want to use the package(s) you are installing.)

    \* You can set up the installation directory to support ".pth" files by
    using one of the approaches described here:

    https://pythonhosted.org/setuptools/easy_install.html#custom-installation-locations

    Please make the appropriate changes for your system and try again.
    running install
    Checking .pth file support in /usr/local/lib/python2.7/site-packages/
    /usr/bin/python2.7 -E -c pass
    TEST FAILED: /usr/local/lib/python2.7/site-packages/ does NOT support .pth files
    error: bad install directory or PYTHONPATH

    You are attempting to install a package to a directory that is not
    on PYTHONPATH and which Python does not read ".pth" files from.  The
    installation directory you specified (via --install-dir, --prefix, or
    the distutils default setting) was:

        /usr/local/lib/python2.7/site-packages/

    and your PYTHONPATH environment variable currently contains:

        ''

    Here are some of your options for correcting the problem:

    \* You can choose a different installation directory, i.e., one that is
    on PYTHONPATH or supports .pth files

    \* You can add the installation directory to the PYTHONPATH environment
    variable.  (It must then also be on PYTHONPATH whenever you run
    Python and want to use the package(s) you are installing.)

    \* You can set up the installation directory to support ".pth" files by
    using one of the approaches described here:

    https://pythonhosted.org/setuptools/easy_install.html#custom-installation-locations

    Please make the appropriate changes for your system and try again.
    running install
    Checking .pth file support in /usr/local/lib/python2.7/site-packages/
    /usr/bin/python2.7 -E -c pass
    TEST FAILED: /usr/local/lib/python2.7/site-packages/ does NOT support .pth files
    error: bad install directory or PYTHONPATH

    You are attempting to install a package to a directory that is not
    on PYTHONPATH and which Python does not read ".pth" files from.  The
    installation directory you specified (via --install-dir, --prefix, or
    the distutils default setting) was:

        /usr/local/lib/python2.7/site-packages/

    and your PYTHONPATH environment variable currently contains:

        ''

    Here are some of your options for correcting the problem:

    \* You can choose a different installation directory, i.e., one that is
    on PYTHONPATH or supports .pth files

    \* You can add the installation directory to the PYTHONPATH environment
    variable.  (It must then also be on PYTHONPATH whenever you run
    Python and want to use the package(s) you are installing.)

    \* You can set up the installation directory to support ".pth" files by
    using one of the approaches described here:

    https://pythonhosted.org/setuptools/easy_install.html#custom-installation-locations

    Please make the appropriate changes for your system and try again.
    -- Installing: /usr/local/bin/ceph-bluestore-tool
    -- Set runtime path of "/usr/local/bin/ceph-bluestore-tool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_jerasure.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_jerasure_generic.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_jerasure_sse3.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_jerasure_sse4.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_lrc.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_shec.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_shec_generic.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_shec_sse3.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_shec_sse4.so
    -- Installing: /usr/local/lib64/ceph/erasure-code/libec_isa.so
    -- Installing: /usr/local/bin/ceph_kvstorebench
    -- Set runtime path of "/usr/local/bin/ceph_kvstorebench" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_rgw_meta
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_rgw_meta" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_bench_log
    -- Set runtime path of "/usr/local/bin/ceph_bench_log" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_multi_stress_watch
    -- Set runtime path of "/usr/local/bin/ceph_multi_stress_watch" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_objectstore_bench
    -- Set runtime path of "/usr/local/bin/ceph_objectstore_bench" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_omapbench
    -- Set runtime path of "/usr/local/bin/ceph_omapbench" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_perf_local
    -- Set runtime path of "/usr/local/bin/ceph_perf_local" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_xattr_bench
    -- Set runtime path of "/usr/local/bin/ceph_xattr_bench" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_filejournal
    -- Set runtime path of "/usr/local/bin/ceph_test_filejournal" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_stress_watch
    -- Set runtime path of "/usr/local/bin/ceph_test_stress_watch" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_admin_socket_output
    -- Set runtime path of "/usr/local/bin/ceph_test_admin_socket_output" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_smalliobenchrbd
    -- Set runtime path of "/usr/local/bin/ceph_smalliobenchrbd" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_smalliobench
    -- Set runtime path of "/usr/local/bin/ceph_smalliobench" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_smalliobenchfs
    -- Set runtime path of "/usr/local/bin/ceph_smalliobenchfs" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_smalliobenchdumb
    -- Set runtime path of "/usr/local/bin/ceph_smalliobenchdumb" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_tpbench
    -- Set runtime path of "/usr/local/bin/ceph_tpbench" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_hello
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_hello" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_lock
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_lock" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_log
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_log" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_numops
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_numops" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_sdk
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_sdk" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_journal
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_journal" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_rbd
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_rbd" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_refcount
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_refcount" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_rgw
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_rgw" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_cls_lua
    -- Set runtime path of "/usr/local/bin/ceph_test_cls_lua" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_erasure_code_benchmark
    -- Set runtime path of "/usr/local/bin/ceph_erasure_code_benchmark" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_erasure_code
    -- Set runtime path of "/usr/local/bin/ceph_erasure_code" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_trim_caps
    -- Set runtime path of "/usr/local/bin/ceph_test_trim_caps" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_libcephfs
    -- Set runtime path of "/usr/local/bin/ceph_test_libcephfs" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_libcephfs_access
    -- Set runtime path of "/usr/local/bin/ceph_test_libcephfs_access" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_aio
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_aio" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_c_read_operations
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_c_read_operations" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_c_write_operations
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_c_write_operations" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_cmd
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_cmd" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_io
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_io" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_list
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_list" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_lock
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_lock" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_misc
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_misc" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_pool
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_pool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_service
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_service" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_snapshots
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_snapshots" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_stat
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_stat" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_tier
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_tier" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_api_watch_notify
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_api_watch_notify" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_striper_api_striping
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_striper_api_striping" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_striper_api_io
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_striper_api_io" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_striper_api_aio
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_striper_api_aio" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_librbd_fsx
    -- Set runtime path of "/usr/local/bin/ceph_test_librbd_fsx" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_librbd
    -- Set runtime path of "/usr/local/bin/ceph_test_librbd" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_librbd_api
    -- Set runtime path of "/usr/local/bin/ceph_test_librbd_api" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_mon_workloadgen
    -- Set runtime path of "/usr/local/bin/ceph_test_mon_workloadgen" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_async_driver
    -- Set runtime path of "/usr/local/bin/ceph_test_async_driver" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_msgr
    -- Set runtime path of "/usr/local/bin/ceph_test_msgr" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_async_networkstack
    -- Set runtime path of "/usr/local/bin/ceph_test_async_networkstack" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_perf_msgr_server
    -- Set runtime path of "/usr/local/bin/ceph_perf_msgr_server" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_perf_msgr_client
    -- Set runtime path of "/usr/local/bin/ceph_perf_msgr_client" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_perf_objectstore
    -- Set runtime path of "/usr/local/bin/ceph_perf_objectstore" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_objectstore
    -- Set runtime path of "/usr/local/bin/ceph_test_objectstore" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_keyvaluedb
    -- Set runtime path of "/usr/local/bin/ceph_test_keyvaluedb" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_filestore_idempotent_sequence
    -- Set runtime path of "/usr/local/bin/ceph_test_filestore_idempotent_sequence" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados
    -- Set runtime path of "/usr/local/bin/ceph_test_rados" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_objectcacher_stress
    -- Set runtime path of "/usr/local/bin/ceph_test_objectcacher_stress" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rbd_mirror
    -- Set runtime path of "/usr/local/bin/ceph_test_rbd_mirror" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rbd_mirror_random_write
    -- Set runtime path of "/usr/local/bin/ceph_test_rbd_mirror_random_write" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_delete_pools_parallel
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_delete_pools_parallel" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_list_parallel
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_list_parallel" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_open_pools_parallel
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_open_pools_parallel" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_test_rados_watch_notify
    -- Set runtime path of "/usr/local/bin/ceph_test_rados_watch_notify" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_snappy.so.2.0.0
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_snappy.so.2
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_snappy.so
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_zlib.so.2.0.0
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_zlib.so.2
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_zlib.so
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_zstd.so.2.0.0
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_zstd.so.2
    -- Installing: /usr/local/lib64/ceph/compressor/libceph_zstd.so
    -- Set runtime path of "/usr/local/lib64/ceph/compressor/libceph_zstd.so.2.0.0" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/rados
    -- Set runtime path of "/usr/local/bin/rados" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_scratchtool
    -- Set runtime path of "/usr/local/bin/ceph_scratchtool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_scratchtoolpp
    -- Set runtime path of "/usr/local/bin/ceph_scratchtoolpp" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_radosacl
    -- Set runtime path of "/usr/local/bin/ceph_radosacl" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-osdomap-tool
    -- Set runtime path of "/usr/local/bin/ceph-osdomap-tool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-monstore-tool
    -- Set runtime path of "/usr/local/bin/ceph-monstore-tool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib64/ceph/ceph-monstore-update-crush.sh
    -- Installing: /usr/local/bin/ceph-objectstore-tool
    -- Set runtime path of "/usr/local/bin/ceph-objectstore-tool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-client-debug
    -- Set runtime path of "/usr/local/bin/ceph-client-debug" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-kvstore-tool
    -- Set runtime path of "/usr/local/bin/ceph-kvstore-tool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-conf
    -- Set runtime path of "/usr/local/bin/ceph-conf" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/crushtool
    -- Set runtime path of "/usr/local/bin/crushtool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/monmaptool
    -- Set runtime path of "/usr/local/bin/monmaptool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/osdmaptool
    -- Set runtime path of "/usr/local/bin/osdmaptool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_psim
    -- Set runtime path of "/usr/local/bin/ceph_psim" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-authtool
    -- Set runtime path of "/usr/local/bin/ceph-authtool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/cephfs-journal-tool
    -- Set runtime path of "/usr/local/bin/cephfs-journal-tool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/cephfs-table-tool
    -- Set runtime path of "/usr/local/bin/cephfs-table-tool" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/cephfs-data-scan
    -- Set runtime path of "/usr/local/bin/cephfs-data-scan" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/rbd
    -- Set runtime path of "/usr/local/bin/rbd" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/rbd-mirror
    -- Set runtime path of "/usr/local/bin/rbd-mirror" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/rbd-nbd
    -- Set runtime path of "/usr/local/bin/rbd-nbd" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib64/ceph/crypto/libceph_crypto_isal.so.1.0.0
    -- Installing: /usr/local/lib64/ceph/crypto/libceph_crypto_isal.so.1
    -- Installing: /usr/local/lib64/ceph/crypto/libceph_crypto_isal.so
    -- Set runtime path of "/usr/local/lib64/ceph/crypto/libceph_crypto_isal.so.1.0.0" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/etc/bash_completion.d/ceph
    -- Installing: /usr/local/etc/bash_completion.d/rados
    -- Installing: /usr/local/etc/bash_completion.d/rbd
    -- Installing: /usr/local/etc/bash_completion.d/radosgw-admin
    -- Installing: /usr/local/lib64/librbd.so.1.12.0
    -- Installing: /usr/local/lib64/librbd.so.1
    -- Installing: /usr/local/lib64/librbd.so
    -- Set runtime path of "/usr/local/lib64/librbd.so.1.12.0" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/rbd-fuse
    -- Set runtime path of "/usr/local/bin/rbd-fuse" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/rbd-replay
    -- Set runtime path of "/usr/local/bin/rbd-replay" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/rbd-replay-prep
    -- Set runtime path of "/usr/local/bin/rbd-replay-prep" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib64/rados-classes/libcls_kvs.so.1.0.0
    -- Installing: /usr/local/lib64/rados-classes/libcls_kvs.so.1
    -- Installing: /usr/local/lib64/rados-classes/libcls_kvs.so
    -- Installing: /usr/local/bin/ceph_rgw_jsonparser
    -- Set runtime path of "/usr/local/bin/ceph_rgw_jsonparser" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph_rgw_multiparser
    -- Set runtime path of "/usr/local/bin/ceph_rgw_multiparser" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/radosgw
    -- Set runtime path of "/usr/local/bin/radosgw" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/radosgw-admin
    -- Set runtime path of "/usr/local/bin/radosgw-admin" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/radosgw-es
    -- Set runtime path of "/usr/local/bin/radosgw-es" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/radosgw-token
    -- Set runtime path of "/usr/local/bin/radosgw-token" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/radosgw-object-expirer
    -- Set runtime path of "/usr/local/bin/radosgw-object-expirer" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/lib64/librgw.so.2.0.0
    -- Installing: /usr/local/lib64/librgw.so.2
    -- Installing: /usr/local/lib64/librgw.so
    -- Set runtime path of "/usr/local/lib64/librgw.so.2.0.0" to "/usr/local/lib64/ceph"
    -- Installing: /usr/local/bin/ceph-brag
    -- Installing: /usr/local/share/man/man8/ceph-syn.8
    -- Installing: /usr/local/share/man/man8/ceph-conf.8
    -- Installing: /usr/local/share/man/man8/ceph.8
    -- Installing: /usr/local/share/man/man8/ceph-authtool.8
    -- Installing: /usr/local/share/man/man8/ceph-kvstore-tool.8
    -- Installing: /usr/local/share/man/man8/rados.8
    -- Installing: /usr/local/share/man/man8/ceph-post-file.8
    -- Installing: /usr/local/share/man/man8/ceph-dencoder.8
    -- Installing: /usr/local/share/man/man8/ceph-deploy.8
    -- Installing: /usr/local/share/man/man8/crushtool.8
    -- Installing: /usr/local/share/man/man8/ceph-run.8
    -- Installing: /usr/local/share/man/man8/mount.ceph.8
    -- Installing: /usr/local/share/man/man8/ceph-create-keys.8
    -- Installing: /usr/local/share/man/man8/ceph-rest-api.8
    -- Installing: /usr/local/share/man/man8/ceph-debugpack.8
    -- Installing: /usr/local/share/man/man8/ceph-clsinfo.8
    -- Installing: /usr/local/share/man/man8/ceph-detect-init.8
    -- Installing: /usr/local/share/man/man8/ceph-disk.8
    -- Installing: /usr/local/share/man/man8/ceph-volume.8
    -- Installing: /usr/local/share/man/man8/ceph-volume-systemd.8
    -- Installing: /usr/local/share/man/man8/ceph-osd.8
    -- Installing: /usr/local/share/man/man8/osdmaptool.8
    -- Installing: /usr/local/share/man/man8/ceph-bluestore-tool.8
    -- Installing: /usr/local/share/man/man8/ceph-mon.8
    -- Installing: /usr/local/share/man/man8/monmaptool.8
    -- Installing: /usr/local/share/man/man8/ceph-mds.8
    -- Installing: /usr/local/share/man/man8/librados-config.8
    -- Installing: /usr/local/share/man/man8/ceph-fuse.8
    -- Installing: /usr/local/share/man/man8/rbd-fuse.8
    -- Installing: /usr/local/share/man/man8/radosgw.8
    -- Installing: /usr/local/share/man/man8/radosgw-admin.8
    -- Installing: /usr/local/share/man/man8/ceph-rbdnamer.8
    -- Installing: /usr/local/share/man/man8/rbd-mirror.8
    -- Installing: /usr/local/share/man/man8/rbd-replay-many.8
    -- Installing: /usr/local/share/man/man8/rbd-replay-prep.8
    -- Installing: /usr/local/share/man/man8/rbd-replay.8
    -- Installing: /usr/local/share/man/man8/rbdmap.8
    -- Installing: /usr/local/share/man/man8/rbd.8
    -- Installing: /usr/local/share/man/man8/rbd-nbd.8
    ```

