---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Montando o {{site.data.keyword.filestorage_short}} no CoreOS

O CoreOS é uma distribuição poderosa do Linux construída para fazer implementações grandes e escaláveis em uma infraestrutura variada simples de gerenciar. Com base em uma construção do S.O. Chrome, o CoreOS mantém um sistema host leve e usa contêineres do Docker para todos os aplicativos.

## Montando o armazenamento móvel

Todos os arquivos de montagem secundários vão no diretório `/etc/systemd/system`, pois as montagens de nível do sistema estão em um diretório que é somente leitura no CoreOS. Primeiro, deve-se criar um arquivo `MOUNTPOINT.mount`. A seção **Onde** do arquivo `.mount` deve corresponder ao nome do arquivo. Quando o ponto de montagem não está diretamente fora de `/`, deve-se nomear o arquivo usando a sintaxe `path-to-mount.mount`. Por exemplo, se você desejar montar a unidade de armazenamento móvel para `/mnt/www`, dê o nome `mnt-www.mount` ao arquivo.

É possível usar `fdisk` ou `parted` para criar a partição. Certifique-se de que o sistema de arquivos que você cria corresponda ao que está listado no arquivo `.mount` ou o serviço falhará ao ser iniciado.


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


O CoreOS usa `systemd`, portanto, para fazer com que o ponto de montagem sobreviva a uma reinicialização, deve-se ativar o arquivo `*.mount`. Se você usar a sinalização `--now`, a partição será montada imediatamente e configurada para iniciar na inicialização.

```
$ systemctl enable --now mnt-www.mount
```
{:pre}

## Montando o NFS/{{site.data.keyword.filestorage_short}}

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

## Montando o NAS/CIFS

Montar um compartilhamento do CIFS não é suportado nativamente no CoreOS, mas há uma solução alternativa fácil para permitir que o sistema host monte compartilhamentos do NAS. É possível usar um contêiner para construir o módulo `mount.cfis` e, em seguida, copiá-lo para o sistema CoreOS

No sistema CoreOS, execute o seguinte para fazer download e soltar em um contêiner do Fedora.

```
docker run -t -i -v /tmp:/host_tmp fedora /bin/bash
```
{:pre}

Quando você estiver no contêiner, execute o seguinte para construir o utilitário CIFS.

```
dnf groupinstall -y "Development Tools" "Development Libraries"
dnf install -y tar
dnf install -y bzip2
curl https://ftp.samba.org/pub/linux-cifs/cifs-utils/cifs-utils-6.4.tar.bz2 | bunzip2 -c - | tar -xvf -
cd cifs-utils-6.4/
./configure && make
cp mount.cifs /host_tmp/
```
{:codeblock}

Agora que o arquivo `mount.cifs` foi copiado para o host, é possível sair do contêiner do docker inserindo o comando `exit` ou pressionando **Ctrl + d**. Quando você estiver de volta no sistema CoreOS, será possível montar o compartilhamento do CIFS com o comando a seguir:
```
/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount
```
{:pre}

## Montando o ISCSI

Isso não é suportado atualmente no CoreOS, mas está na fila para uma liberação futura. - https://github.com/coreos/bugs/issues/634
