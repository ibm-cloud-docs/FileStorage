---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Montando o {{site.data.keyword.filestorage_short}} no Linux
{: #mountingLinux}

Primeiro, certifique-se de que o host que acessará o volume do {{site.data.keyword.filestorage_full}} esteja autorizado por meio do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.

1. Na página de listagem do {{site.data.keyword.filestorage_short}}, clique no link **Ações** que está associado ao novo compartilhamento e clique em **Autorizar host**.
2. Selecione o host ou os hosts na lista e clique em **Enviar**. Essa ação autoriza o host a acessar o compartilhamento.

Como alternativa, é possível autorizar os hosts por meio do SLCLI.
```
# slcli file access-authorize --help
Usage: slcli file access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  -s, --subnet-id TEXT      The id of one SoftLayer_Network_Subnet to
                            authorize
  --help                    Show this message and exit.
```

## Montando o compartilhamento do {{site.data.keyword.filestorage_short}}

Use estas instruções para conectar uma instância de Cálculo do {{site.data.keyword.BluSoftlayer_full}} baseada em Linux a um compartilhamento do Network file system (NFS) . O exemplo é baseado no Red Hat Enterprise Linux 6. As etapas podem ser ajustadas para outras distribuições do Linux de acordo com a documentação do fornecedor do sistema operacional (OS).

O ponto de montagem da instância de armazenamento de arquivo pode ser obtido por meio da página de listagem do {{site.data.keyword.filestorage_short}} ou por meio de uma chamada API - `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

1. Instale as ferramentas necessárias.
   ```
   # yum -y install nfs-utils nfs-utils-lib
   ```
   {:pre}

2. Monte o compartilhamento remoto.
   ```
   # mount -t "nfs version" -o "options" <mount_point> /mnt
   ```

   Exemplo
   ```
   # mount -t nfs4 -o hard,intr
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt
   ```

3. Verifique se a montagem foi bem-sucedida.
   ```
   # df -h
   Filesystem Size Used Avail Use% Mounted on
   /dev/xvda2 25G 1.4G 22G 6% /
   tmpfs 1.9G 0 1.9G 0% /dev/shm
   /dev/xvda1 97M 51M 42M 55%
   ```

4. Acesse o ponto de montagem e os arquivos de leitura/gravação.
   ```
   # touch /mnt/test
   # ls -la /mnt
   total 12
   drwxr-xr-x 2 nobody nobody 4096 Sep 8 15:52 .
   dr-xr-xr-x. 22 root root 4096 Sep 8 14:30 ..
   -rw-r--r-- 1 nobody nobody 0 Sep 8 15:52 test
   ```

   Os arquivos criados pela raiz têm a propriedade de `nobody:nobody`. Para exibir a propriedade corretamente, o `idmapd.conf` precisa ser atualizado com as configurações de domínio corretas. Consulte a seção [Como implementar no_root_squash para o NFS](#norootsquash).
   {:tip}

5. Monte o compartilhamento remoto no início. Para concluir a configuração, edite a tabela de sistemas de arquivos (`/etc/fstab`) para incluir o compartilhamento remoto na lista de entradas montadas automaticamente na inicialização:

   ```
   (hostname):/(username) /mnt "nfs version" "options" 0 0
   ```

   Exemplo

   ```
   nfsdal0501a.service.softlayer.com:/IBM01SV278685_7 /mnt nfs4 defaults,hard,intr 0 0
   ```

6. Verifique se o arquivo de configuração não tem erros.

   ```
   # mount -fav
   ```
   {:pre}

   Se o comando for concluído sem erros, sua configuração estará completa.

   Se você estiver usando o NFS 4.1, inclua `sec=sys` no comando de montagem para evitar problemas de propriedade do arquivo.
   {:tip}


## Implementando  ` no_root_squash `  para NFS (opcional)
{: #norootsquash}

A configuração de `no_root_squash` permite que os clientes raiz retenham permissões raiz no compartilhamento do NFS.
- Para o NFSv3, os clientes não precisam fazer nada; `no_root_squash` funciona.
- Para o NFSv4, é necessário configurar o domínio nfsv4 como: `slnfsv4.com` e iniciar `rpcidmapd` ou um serviço semelhante usado por seu S.O.

Exemplo

1. No host, defina a configuração de domínio em `/etc/idmapd.conf`.

   ```
   #vi /etc/idmapd.conf
   [General]
   #Verbosity = 0
   #The following should be set to the local NFSv4 domain name
   #The default is the host's DNS domain name.
   Domain = slnfsv4.com
   [Mapping]
   Nobody-User = nobody
   Nobody-Group = nobody
   ```

2. Execute `nfsidmap -c`.
3. Inicie o `rpcidmapd`.
   ```
   # /etc/init.d/rpcidmapd start
   Starting RPC idmapd: [ OK ]
   ```
