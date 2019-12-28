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
## 参数说明
【1】挂载授权 IP 说明：

|配置 |说明|
|  ----  | ----  |
|授权某个IP  |192.168.1.1|
|授权某个网段  |192.168.1.0/24|
|授权某个域名 | www.baidu.com|
|授权某个域   |*.baidu.com|
|授权所有    |*|


【2】授权参数：

|参数 | 说明|
|  ----  | ----  |
|ro | 目录只读|
|rw | 目录可读写|
|all_squash | 将远程访问的所有普通用户和组都映射为匿名（nfsnobody）|
|no_all_squash  | 与 all_squash 取反（默认设置）|
|root_squash |将 root 用户及所属组都映射为匿名用户或用户组（默认设置）|
|no_root_squash  |与 root_squash 取反|
|anonuid=xxx |将远程访问的所有用户都映射为匿名用户，并指定该用户为本地用户（UID=xxx）|
|anongid=xxx| 将远程访问的所有用户组都映射为匿名用户组，并指定该匿名用户组为本地用户组（GID=xxx）|
|secure | 限制客户端只能从小于 1024 的 tcp/ip 端口连接 nfs 服务器（默认设置）|
|insecure   | 允许客户端从大于 1024 的 tcp/ip 端口连接服务器|
|sync   | 将数据同步写入内存缓冲区与磁盘中，效率低，但可以保证数据的一致性|
|async   |将数据先保存在内存缓冲区中，必要时才写入磁盘|
|wdelay | 检查是否有相关的写操作，如果有则将这些写操作一起执行，这样可以提高效率（默认设置）|
|no_wdelay |  若有写操作则立即执行，应与 sync 配合使用|
|subtree_check  | 若目录是一个子目录，则 nfs 服务器将检查其父目录的权限(默认设置)|
|no_subtree_check   | 即使目录是一个子目录，nfs 服务器也不检查其父目录的权限，这样可以提高效率|
