---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-08"

keywords: File Storage, file storage, NFS, mounting volume in Container Linux, CoreOS

subcollection: FileStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# Montando o {{site.data.keyword.filestorage_short}} no Container Linux
{: #mountingCoreOS}

O Container Linux by CoreOS é um sistema operacional leve de software livre baseado no kernel do Linux. Ele foi projetado para fornecer infraestrutura para implementações em cluster. Como um sistema operacional,
o Container Linux fornece a funcionalidade mínima requerida para implementar aplicativos dentro de
contêineres de software, juntamente com mecanismos integrados para descoberta de serviço e compartilhamento de configuração. Para obter mais informações, veja [Montando o armazenamento](https://coreos.com/os/docs/latest/mounting-storage.html)

Todos os arquivos de montagem secundários vão no diretório `/etc/systemd/system`, pois as montagens de nível do sistema estão em um diretório que é somente leitura. Primeiro, deve-se criar um arquivo `MOUNTPOINT.mount`. A seção **Onde** do arquivo `.mount` deve corresponder ao nome do arquivo. Quando o ponto de montagem não está diretamente fora de `/`, deve-se nomear o arquivo usando a sintaxe `path-to-mount.mount`. Por exemplo, se você desejar montar a unidade de armazenamento móvel para `/mnt/www`, dê o nome `mnt-www.mount` ao arquivo.

É possível usar `fdisk` ou `parted` para criar a partição. Certifique-se de que o sistema de arquivos que você cria corresponda ao que está listado no arquivo `.mount` ou o serviço falhará ao iniciar. Como a montagem é NFS, é possível especificar mais opções usando a linha `Options=` no arquivo de montagem.

No exemplo a seguir, o NFS é configurado para ser montado em `/data/www`. O ponto de montagem do NFS da instância do {{site.data.keyword.filestorage_short}} pode ser obtido na página de listagem do {{site.data.keyword.filestorage_short}} ou por meio de uma chamada API `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

```
$ cat data-www.mount [Unidade] Descrição = Montar para o armazenamento de contêiner

[Mount]
What=<nfs_mount_point>
Where=/data/www
Type=nfs
Options=vers=3,sec=sys,noauto

[Install] WantedBy = multi-user.target
```
{:codeblock}

Em seguida, ative a montagem e verifique se ela está montada de forma adequada.

```
systemctl enable --now /etc/systemd/system/data-www.mount

cluster1 ~ # mount |grep data
<nfs_mount_point> on /data/www type nfs3 (rw,relatime,vers=3.0,rsize=65536,wsize=65536,namlen=255,hard,proto=tcp,port=0,timeo=600,retrans=2,sec=sys,clientaddr=10.81.x.x,local_lock=none,addr=10.1.x.x)
```
{:codeblock}

Para obter mais informações, consulte a documentação do [`systemd mount`](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)
