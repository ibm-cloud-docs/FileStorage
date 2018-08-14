---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# Montando o NFS/{{site.data.keyword.filestorage_short}} no CentOS

O processo para montagem do {{site.data.keyword.filestorage_full}} no CentOS 7 é semelhante ao processo [Montando o {{site.data.keyword.filestorage_short}} no RHEL 6](accessing-file-storage-linux.html). No entanto, como a montagem é NFS, é possível especificar algumas opções adicionais usando a linha `Options=` no arquivo de montagem. No exemplo a seguir, o NFS está configurado para montagem em `/data/www`. 

>**Nota** - O ponto de montagem do NFS da instância do {{site.data.keyword.filestorage_short}} pode ser obtido da página de listagem do {{site.data.keyword.filestorage_short}} ou por meio de uma chamada API - `SoftLayer_Network_Storage::getNetworkMountAddress()`.

```
$ cat data-www.mount [Unidade] Descrição = Montar para o armazenamento de contêiner

[Mount] What=<nfs_mount_point> Where=/data/www Type=nfs Options=vers=4,sec=sys,noauto

[Install] WantedBy = multi-user.target
```
{:codeblock}

Em seguida, ative a montagem e verifique se ela está montada de forma adequada.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data <nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}
