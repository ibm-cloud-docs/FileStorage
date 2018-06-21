---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}

# Montando o {{site.data.keyword.filestorage_short}} no CoreOS

CoreOS é uma distribuição Linux poderosa construída para fazer implementações grandes e escaláveis em uma infraestrutura variada simples de gerenciar. Com base em uma construção do S.O. Chrome, o CoreOS mantém um sistema host leve e usa contêineres do Docker para todos os aplicativos.

## Montando o armazenamento móvel

Todos os arquivos de montagem secundários vão no diretório `/etc/systemd/system`, pois as montagens de nível do sistema estão em um diretório que é somente leitura no CoreOS. Você criará um arquivo `MOUNTPOINT.mount`. A seção **Where** do arquivo .mount deve corresponder ao nome do arquivo. Se o ponto de montagem não está diretamente fora do `/`, deve-se nomear o arquivo usando a sintaxe a seguir: `path-to-mount.mount`. Como é possível ver no exemplo a seguir, desejamos montar a unidade de armazenamento móvel em `/mnt/www`, portanto nomeamos o arquivo como `mnt-www.mount`.

Deve-se usar `fdisk` ou `parted` para criar a partição e assegurar que o sistema de arquivos criado corresponda a um listado no arquivo `.mount` ou então o serviço falhará ao ser iniciado.


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


O CoreOS usa `systemd`, portanto, para fazer o ponto de montagem sobreviver a uma reinicialização, deve-se ativar o arquivo `*.mount`. Se você usar a sinalização `--now`, a partição será montada imediatamente e configurada para ser iniciada na inicialização.

```
$ systemctl enable --now mnt-www.mount
```
{:pre}

## Montando o NFS/{{site.data.keyword.filestorage_short}}

O processo para montar nosso {{site.data.keyword.filestorage_short}} é praticamente o mesmo, mas como a montagem é NFS, podemos especificar algumas opções adicionais usando a linha Options= no arquivo de montagem. No exemplo a seguir, estamos configurando o NFS para montar em `/data/www`. Observe que o ponto de montagem do NFS da instância do {{site.data.keyword.filestorage_short}} pode ser obtido na página de listagem do {{site.data.keyword.filestorage_short}} ou por meio de uma chamada API `SoftLayer_Network_Storage::getNetworkMountAddress()`.

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
{:codeblock}
 
## Montando o NAS/Cifs

Montar um compartilhamento do cifs não é suportado nativamente no CoreOS, mas há uma solução alternativa fácil para permitir que o sistema host monte compartilhamentos do NAS. É possível usar um contêiner para construir o módulo `mount.cfis` e, em seguida, copiá-lo no sistema CoreOS
 
No sistema CoreOS, execute o seguinte para fazer download e soltar em um contêiner Fedora: 
```
docker run -t -i -v /tmp:/host_tmp fedora /bin/bash
```
{:pre}
 
Quando estiver no contêiner, execute o seguinte para construir o utilitário cifs
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
 
Agora que o arquivo mount.cifs foi copiado para o host, é possível sair do contêiner do docker inserindo o comando `exit` ou pressionando **ctrl+d**. Quando você estiver de volta no sistema CoreOS, será possível montar o compartilhamento do CIFS com o comando a seguir: 
```
/tmp/mount.cifs //nasXXX.service.softlayer.com/USERNAME -o username=USERNAME,password=PASSWORD /path/to/mount
```
{:pre}
 
## Montando o ISCSI

Isso não é suportado atualmente no CoreOS, mas está na fila para uma liberação futura - https://github.com/coreos/bugs/issues/634
