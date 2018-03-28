---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Acessando o {{site.data.keyword.filestorage_short}} no Linux

Antes de iniciar, certifique-se de que o host que acessa o volume do {{site.data.keyword.filestorage_full}} foi autorizado por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. Na página de listagem do {{site.data.keyword.filestorage_short}}, clique em **Ações** associadas ao compartilhamento recentemente provisionado e clique em **Autorizar host**.
2. Selecione os hosts desejados na lista e clique em **Enviar**; isso autoriza os hosts a acessarem o compartilhamento.

## Montando o compartilhamento do {{site.data.keyword.filestorage_short}}

A seguir estão as etapas necessárias para conectar uma instância do {{site.data.keyword.BluSoftlayer_full}} Compute com base no Linux a um compartilhamento do Network File System (NFS). O exemplo é baseado no Red Hat Enterprise Linux 6. As etapas podem ser ajustadas para outras distribuições do Linux de acordo com a documentação do fornecedor do sistema operacional (OS).

**Nota:** o ponto de montagem da instância de armazenamento de arquivo pode ser obtido na página de listagem do {{site.data.keyword.filestorage_short}} ou por meio de chamada para a API - SoftLayer_Network_Storage::getNetworkMountAddress().

1. Instale os pacotes/ferramentas necessários.

    `[root@TEST-RHEL6]# yum -y install nfs-utils nfs-utils-lib
    `
2. Monte o compartilhamento remoto
    `[root@TEST-RHEL6]# mount -t "nfs version" -o "options" <mount_point> /mnt`
    
    Aqui está um exemplo de montagem do compartilhamento remoto para uma instância de armazenamento.
    
    `[root@TEST-RHEL6 mnt]# mount -t nfs4 -o hard,intr`
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt`
 
3. Verifique se a montagem foi bem-sucedida.

    `[root@TEST-RHEL6]# df -h`
    
    `Filesystem Size Used Avail Use% Mounted on`
    
    `/dev/xvda2 25G 1.4G 22G 6% /`
    
    `tmpfs 1.9G 0 1.9G 0% /dev/shm`
    
    `/dev/xvda1 97M 51M 42M 55%`
    
4. Navegue para o ponto de montagem e arquivos de leitura/gravação.

    `[root@TEST-RHEL6]# touch /mnt/test`
    
    `[root@TEST-RHEL6]# ls -la /mnt`
    
    `total 12
`
    
    `drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .`
    
    `dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..`
    
    `-rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test`

    **Nota:** os arquivos criados por raiz terão a propriedade de nobody:nobody. Para exibir a propriedade corretamente, o idmapd.conf precisa ser atualizado com as configurações de domínio correto. Veja “Como implementar no_root_squash para NFS” abaixo.
    
5. Monte o compartilhamento remoto na inicialização. Para concluir a configuração, edite a tabela de sistemas de arquivos /etc/fstab para incluir o compartilhamento remoto na lista de entradas que serão montadas automaticamente na inicialização:

    `(hostname):/(username) /mnt "nfs version" "options" 0 0`
    
    Usando a instância da montagem do exemplo de compartilhamento remoto, a entrada seria:
    
    `nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0`
    
6.  Verifique se não há erros com o arquivo de configuração.

    `[root@TEST-RHEL6 /]# mount -fav`
    
    Se o comando for concluído sem erros, sua configuração estará completa.

**Nota:** se usar o NFS 4.1, inclua 'sec=sys' no comando de montagem para evitar problemas de propriedade do arquivo.

 
## Como implementar no_root_squash para NFS (opcional)

Configurar no_root_squash permite que os clientes raiz retenham as permissões raiz no compartilhamento do NFS. Para NFSv3, não há nada que os clientes precisarão fazer; no_root_squash deve simplesmente funcionar.
Para NFSv4, será necessário configurar o domínio nfsv4 para: slnfsv4.com e iniciar o rpcidmapd ou um serviço semelhante dependendo da versão do S.O.

Aqui está um exemplo:

1. No host, defina a configuração de domínio em /etc/idmapd.conf.

    `#vi /etc/idmapd.conf`
    
    `[General]`
    
    `#Verbosity = 0`
    
    `#The following should be set to the local NFSv4 domain name`
    
    `#The default is the host's DNS domain name.`
    
    `Domain = slnfsv4.com`
    
    `[Mapping]`
    
    `Nobody-User = nobody`
    
    `Nobody-Group = nobody`
    
2. Execute nfsidmap -c.
3. Inicie o rpcidmapd.

   ` # /etc/init.d/rpcidmapd start`
   
   ` Starting RPC idmapd: [ OK ]`
