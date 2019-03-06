---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: FileStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}


# Montando o {{site.data.keyword.filestorage_short}} no Container Linux
{: #mountingCoreOS}

O Container Linux by CoreOS é um sistema operacional leve de software livre baseado no kernel do Linux. Ele foi projetado para fornecer infraestrutura para implementações em cluster. Como um sistema operacional,
o Container Linux fornece a funcionalidade mínima requerida para implementar aplicativos dentro de
contêineres de software, juntamente com mecanismos integrados para descoberta de serviço e compartilhamento de configuração. Para obter mais informações, veja [Montando o armazenamento ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://coreos.com/os/docs/latest/mounting-storage.html)

## Montando o armazenamento móvel

Todos os arquivos de montagem secundários vão no diretório `/etc/systemd/system`, pois as montagens de nível do sistema estão em um diretório que é somente leitura. Primeiro, deve-se criar um arquivo `MOUNTPOINT.mount`. A seção **Onde** do arquivo `.mount` deve corresponder ao nome do arquivo. Quando o ponto de montagem não está diretamente fora de `/`, deve-se nomear o arquivo usando a sintaxe `path-to-mount.mount`. Por exemplo, se você desejar montar a unidade de armazenamento móvel para `/mnt/www`, dê o nome `mnt-www.mount` ao arquivo.

É possível usar `fdisk` ou `parted` para criar a partição. Certifique-se de que o sistema de arquivos que você cria corresponda ao que está listado no arquivo `.mount` ou o serviço falhará ao iniciar.


```
[Unit]
Descrição = Montar para armazenamento móvel

[Mount]
What=/dev/xvdc1
Where=/mnt/www
Type=ext4

[Install] WantedBy = multi-user.target
```
{:codeblock}


Esse S.O. usa `systemd`, portanto, para fazer com que o ponto de montagem
sobreviva a uma reinicialização, deve-se ativar o arquivo `*.mount`. Se você usar o sinalizador `--now`, a partição será montada imediatamente e configurada para iniciar na inicialização.

```
systemctl enable --now mnt-www.mount
```
{:pre}

Para obter mais informações, consulte a documentação do [`systemd mount` ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)

## Montando o {{site.data.keyword.filestorage_short}}

O processo para montagem do {{site.data.keyword.filestorage_short}} é o mesmo. Como a montagem é NFS, é possível especificar mais opções usando a linha `Options=` no arquivo de montagem.

No exemplo, NFS é configurado para montagem em `/data/www`. O ponto de montagem do NFS da instância do {{site.data.keyword.filestorage_short}} pode ser obtido na página de listagem do {{site.data.keyword.filestorage_short}} ou por meio de uma chamada API `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

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
