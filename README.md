# nfs4-server
Docker image with NFS v4, based on Alpine.

## 快速启动

```
docker run --privileged -d --name=nfs \
    -p 2049:2049/tcp -p 2049:2049/udp \
    -v /tmp:/nfs-share \
    ghostry/nfs4
```

## 可用配置

方式1,使用变量设置共享文件:

- NFS_EXPORT_DIR
- NFS_EXPORT_DOMAIN
- NFS_EXPORT_OPTIONS

启动时会使用变量初始化文件 `/etc/exports` :

`NFS_EXPORT_DIR NFS_EXPORT_DOMAIN(NFS_EXPORT_OPTIONS)`

默认值为:

`/nfs-share *(rw,fsid=0,sync,no_subtree_check,no_auth_nlm,insecure,no_root_squash,crossmnt,no_acl)`

> **这不是安全的选项,请只在安全环境内使用**

方式2,使用exports文件,

```
docker run --privileged -d --name=nfs \
    -p 2049:2049/tcp -p 2049:2049/udp \
    -v /path/exports:/etc/exports:ro \
    -v /tmp:/nfs-share \
    ghostry/nfs4
```

## 客户端挂载

安装客户端

```
sudo apt-get install nfs-common
```

挂载

```
sudo mount -v -t nfs4 127.0.0.1:/ /mnt/nfs
```
