---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# Montando o NFS/{{site.data.keyword.filestorage_short}} no CentOS

O processo para montar nosso {{site.data.keyword.filestorage_full}} no CentOS 7 é praticamente o mesmo que o processo para [Montar o {{site.data.keyword.filestorage_short}} no RHEL 6](accessing-file-storage-linux.html). No entanto, como a montagem é NFS, podemos especificar algumas opções adicionais usando a linha *Options=* no arquivo de montagem. No exemplo a seguir, estamos configurando o NFS para montar em `/data/www`. 

**Nota**: o ponto de montagem do NFS da instância do {{site.data.keyword.filestorage_short}} pode ser obtido na página de listagem do {{site.data.keyword.filestorage_short}} ou por meio de uma chamada API -`SoftLayer_Network_Storage::getNetworkMountAddress()`.

```
$ cat data-www.mount [Unidade] Descrição = Montar para o armazenamento de contêiner

[Mount] What=<nfs_mount_point> Where=/data/www Type=nfs Options=vers=4,sec=sys,noauto

[Install] WantedBy = multi-user.target
```
{:codeblock}

Agora é possível ativar a montagem e verificar se está montada corretamente.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data <nfs_mount_point> on /data/www type nfs4 (rw,relatime,vers=4.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock
